---
title: "The Composable Architecture: My 3 Year Experience"
date: 2024-04-26T11:35:37-06:00
draft: false
tags: ['Architecture', 'Apple Platforms', 'TCA', 'The Composable Architecture', 'Swift', 'SwiftUI']
---
## Introduction
I recently finished a 3 year stint with a company that uses the Composable Architecture ([TCA](https://github.com/pointfreeco/swift-composable-architecture))
from [PointFree](https://pointfree.co). I wanted to write about my experiences with TCA and some of
the problems I see with it.

I think Brandon Williams and Stephen Cellis, the creators of TCA, are absolutely brilliant, and what
they have managed to pull off with the creation of TCA is amazing. However, It’s just the two of them,
and nobody, or no thing, is perfect.

## The Start
In February of 2021, I found out that a local company was using TCA to write their software for iOS.
I’m a big fan of functional programming, and had been watching the PointFree videos, so I was very
much interested in TCA and getting a chance to use it. I’m also a fan of the [Elm programming language](https://elm-lang.org),
[The Elm Architecture](https://guide.elm-lang.org/architecture/)(TEA), and I saw TCA as a potential
version of that for iOS apps. This kind of programming is called [functional reactive programming](https://en.wikipedia.org/wiki/Functional_reactive_programming)
and other frameworks that you may have heard of like this are [React](https://react.dev/) and [RxSwift](https://github.com/ReactiveX/RxSwift)

Many iOS developers in Utah are on a Slack group where we regularly discuss things related to the industry.
I reached out to someone I knew that worked at the company and soon had an interview. I interviewed
with 3 of their lead developers. I must have done well because I started working for the company at
the beginning of March 2021. I was on a team that had responsibility for one iOS app out of 3 the
company had on the App Store. It wasn’t long before I ran into my first issue with TCA.

## It’s Complicated
TCA has a steep learning curve. You need to learn about reducers, stores, view stores, environments,
and scoping of state and actions. You can’t just use SwiftUI like you learned from Apple. Every view
needs a store, and if you care about performance, a view store. You need to learn how to use TCA’s top
notch testing abilities, and use a test store. You can’t just use XCTest by itself.

In March of 2021, TCA was at version [0.16.0](https://github.com/pointfreeco/swift-composable-architecture/releases/tag/0.16.0).
Things have improved a lot since then with the latest release ([1.9.x](https://github.com/pointfreeco/swift-composable-architecture/releases/tag/1.9.0)),
but really the main things that have improved are the ergonomics of using the framework, and its performance.
View stores are no longer required with Apple's new [Observation](https://developer.apple.com/documentation/Observation)
framework and `@Observable` macro introduced in iOS 17, but you need to know about TCA’s `@ObservableState`
macro. If you're on iOS 16 or earlier, you need to know about TCA's `withPerceptionTracking`, the `@Perceptible`
macro, and the [Perception](https://github.com/pointfreeco/swift-perception) framework. Environments got
replaced with the [swift-dependencies](https://github.com/pointfreeco/swift-dependencies) framework
and the `@Dependency` macro, and you need to learn how to use that. Macros have improved case
paths and how you refer to scoped actions.

Even with these improvements, you still need to learn a lot to use TCA. You also have to constantly
re-learn things as the framework gets updated and changes. All the changes have been improvements, but
it takes time to learn them, especially when you’re trying to get work done.

## The Churn
TCA changes a lot, significantly. More than the iOS SDK. When I say significantly, I mean that, while
backward compatibility was kept, the new functionality had significant changes that you wanted to use
and would improve the code, but would require significant changes to use.

Over my 3 years, it went from version 0.16.0 to 1.9.2. There were 95 releases over the 3 years, most
of them minor ([semver](https://semver.org/) wise) releases that just added functionality and kept
backward compatibility. Now, we were using a pre-1.0 framework, so you can’t really blame TCA or PointFree,
but if you look at just the releases between 1.0 (July 2023) and 1.9.2 (March 2024), there were 24 releases
in less than a year. Again, most of them minor releases, but with significant additions.

When I left, we were still using 1.6, because when we attempted to migrate to 1.7 with the new Observation
support, we ran into some issues that would require lots of changes and some other tricky issues to
resolve because we were still supporting iOS 15. This meant we couldn’t use Apple’s Observation framework.
Instead, we needed to use PointFree's Perception framework and wrap many views in `withPerceptionTracking`.
This in itself is tricky, as you need to start at the root view and work down the view tree. If you
don’t, you’ll end up with run-time warnings and lots of run-time issues. Using the `@ObservableState`
macro presented another problem, because macros don’t play well with property wrappers, which we were
using in our state. (See Performance Issues).

More on our team structure later, but when you have 8 teams, all with their own code that needs to be
updated at the same time as all the other teams, migrating to a new version is not always easy, especially
if teams are under pressure to deliver features.

## Architectural Issues
I’ve already talked about how complicated TCA is. You can be much more productive with another architecture,
such as MVVM (with additional pieces), and get the same benefits, such as a unidirectional data flow,
easier unit testing, and modular code. See some of my other posts on this, but I intend to delve deeper
into this in future posts.

The other issue is that TCA is built around functional programming and this is in opposition to the iOS
SDK’s object-oriented roots. Yes, Swift and SwiftUI are more functional, but they are built on top of
Objective-C and Cocoa, an object-oriented language and framework. Everything is designed and optimized
around that. TCA is functional and can’t take advantage of that, resulting in performance issues, and
an impedance mismatch with the platform.

Take reducers and actions for example. To me, this is just object-oriented message passing. TCA basically
implements its own message passing and handling. When using TCA, I just wanted to call a method, which
would be the equivalent of sending the action. With MVVM, or another architecture, I would be able to
do just this.

There’s basically no encapsulation. Code can intercept an action sent from anywhere in the reducer hierarchy,
and all the state for a reducer hierarchy has to exist in the root state. You can’t hide a piece of state
from other areas, nor can you hide actions.

Another issue is massive reducers. Just like MVC can lead to Massive View Controllers, I’ve seen TCA
lead to Massive Reducers. Our application reducer was so large, Xcode had problems scrolling through
it, and it wouldn’t give you valid compiler errors. I ended up splitting it up into about 20 separate
files containing extensions to the app reducer. Each extension was for handling a related group of actions.
So, the app reducer was handling at least 20 different responsibilities.

Reducers are mostly just a switch statement with a case for each action. People tend to just put all
the action handling in there without calling out to separate functions on the reducer. Composing reducers
can be a pain (see It’s Complicated), so your usually under pressure developer just throws everything
into that switch statement.

One could argue that this is a skill issue (or laziness), and I would be inclined to agree with you.
We also all know how common an issue like Massive View Controllers is.

## Performance Issues
With our large application, we ran into some performance issues.

First, TCA uses a lot of stack. If you try to debug a TCA application, good luck trying to find where
your code is being called from. The stack is so large and there’s so many TCA framework functions calls
to handle actions. With our large application and large application state, we'd have stack overflow
errors, and would have to increase the stack space allocated to our code. We also resorted to creating
a property wrapper to put references in our application state, which is usually just a struct with value
types.

Next, as mentioned above, processing an action can be quite slow. Each action has to go up through the
entire reducer tree. In a large application, that can be significant. Similar to how Objective-C message
sending could be slow.

Also, because of the lack of encapsulation (i.e. free access to state and actions), and the composition
of reducers, changing some code in one area, would frequently require lots of code to be recompiled,
resulting in slow build times.

## Company Organizational Issues
When I started with the company, I was on a team that had complete control of one app. That app was discontinued,
and when I left, I was on a platform team, supporting 7 other teams, all working on one app. This made
things even more complicated. All 8 teams had to use the same version of TCA, so when we wanted to upgrade,
everyone had to upgrade, due to the Swift Package Manager not allowing multiple version of the same
framework.

Since TCA has no encapsulation, and any parent reducer can get access to any child’s state or actions,
you run into problems with teams reaching into other stores and relying on code they shouldn’t be. Even
if your teams are disciplined, this can just happen because a parent’s state contains all the children
states. That code may change and then the calling code breaks. This happened frequently and we basically
had to build the entire app during our PR checks to make sure nothing got broken.

With this many teams, you need to put some measures in place to deal with these issues. One solution is
to have separate stores for each team. That way each team’s code would be isolated and executing in
their own store. The brilliant [Krzysztof Zabłocki](https://www.merowing.info/) has written about this
and I recommend his site for other ways to deal with TCA’s scaling issues, such as partitioning your
actions into delegate and internal actions. If you use separate stores, you will then need to figure
out how to communicate between stores. One approach would be to have each team (or store) have a TCA
client (i.e. a dependency) that another team could use. This client would, in effect, create a clean
boundary or interface between two teams.

## Company Risks
Let’s talk about the risks a company takes on when choosing a framework like TCA.

First off, TCA is a 3rd party framework. This means Apple doesn’t support it or care about it. Something
Apple does may break it (like a new iOS release) and you may have to wait for the framework to get updated.
The framework can’t take full advantage of the platform like Apple can, because it can’t make use of
internals like Apple can. This is really an issue with TCA because of the performance issues I talked
about.

Next, TCA is primarily the work of 2 guys. 2 brilliant and prolific guys, but 2 guys nonetheless. If
they get bored, injured, change careers, whatever, you could be in some trouble. This is mitigated somewhat
by TCA being open-source and presumably a company and/or the community could take over, but it is still
an issue.

Lastly, a company as large as the one I worked for, really shouldn’t be using an immature framework like
TCA was. I think, now at 1.9.x, TCA is pretty mature. You don’t want to start at 0.16.0. This is what
happens when you have inexperienced developers (most software developers have < 5 years experience,
according to Uncle Bob) wanting to use some cool tech, and managers listening to them, who really should
know better. Some of these managers, even had technical experience.

I asked why TCA was chosen and was told they chose it because they needed an architecture to keep all
the developers from doing whatever they wanted. Their main concern was all the contractors they have
in Eastern Europe. This doesn’t seem like a very good reason to choose a framework to me. There are
other architectures to use, such as MVVM (or MVVM+), that are easier to use and serve the same purpose.
If that is your concern, maybe you should just hire better developers and provide better guidance?
In all fairness, the developers we had in Eastern Europe were pretty good and they didn’t need an architecture
like TCA to keep them in line.

## Conclusion
TCA is an incredible accomplishment, but it has some problems when used in a large application composed
of code from multiple teams. Your mileage may vary and it could work for you, especially if you have a
single team, a relatively small application, or some great disciplined developers. With a large app and
multiple teams you will need to deal with the lack of encapsulation and other scaling issues.

TCA is a 3rd party framework without Apple’s support, and it depends on just 2 brilliant guys that you’re
basically betting your whole code base on. It’s a functional programming based framework that goes against
SwiftUI’s object-oriented heritage and influence. You might be more productive onboarding new developers
and adding features with another architecture and still be able to achieve your desired architectural
discipline with MVVM or Clean Architecture.

In future posts, I will talk about how you can use a simpler architecture to achieve the same goals as
you would want to achieve with TCA, and how to establish clean boundaries between different areas of
your code or between teams. Some of my previous posts have already talked about this, but new posts would
be more specific to SwiftUI, and address clean boundaries.

I’m available for consulting, so if you need help addressing these issues, I can provide suggestions
and guidance.
