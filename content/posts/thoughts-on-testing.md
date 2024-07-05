---
title: "Thoughts on Testing"
date: 2024-07-05T15:18:45-06:00
draft: true
tags: ['Testing']
---
Testing has grown to be a very important part of software development. When I first started my career, I didn’t hear about writing tests until a few years in. Then we started writing tests for all our libraries. When I first got into Cocoa development, testing was not a part of the culture. That slowly changed, and now it is an important part of development on Apple platforms.

In many ways we are still catching up to other development cultures where testing has been much more ingrained in the culture, such as the web development world. Their tools just seem to be more mature, but it is also easier to test something that is based on a stateless protocol like HTTP.

## The Testing Pyramid

Let’s start with the basics. Here’s the famous testing pyramid.

{{<img-center src="/images/testing-pyramid.png" width="400">}}

As you can see, most of your tests should be unit tests. Then fewer still are integration tests, and then very view (if any) UI tests.  Unit tests are supposed to be relatively easy to write and fast to run. As you go up the pyramid, tests become harder to write, and take longer to run (hence, cost more money). Unit tests should be very fast (ideally, less than 1 second for all your tests together), so developers can run them after every change they make while they are writing code.

Pay special attention to the word ‘integration’ vs ‘integrated’. Integration tests verify the communication between modules that depend on each other. Only the communication between the modules is tested. This can be very similar to a unit test, but we mock the unit’s dependencies and verify the unit talks to the dependency correctly. We also simulate responses from the dependency and verify the unit responds correctly.

Integrated tests are where you test a complete set of units with little or no mocks. The idea is to test the complete flow of logic through a set of units, if not a complete app. Later we’ll talk about why this is a bad idea.

## Unit Testing
To me, unit testing is all about testing your non-UI code. As a result, all code that you deem important (i.e. important to the correct functioning of the software) should not be in the UI. If you follow a UI design pattern such as MVVM, your UI code should be dumb, just copying values from the view model to the UI, and relaying user events to the view model. You don’t really need to test it. More about that later.

Also, you don’t need to test 3rd party code, (i.e. code from outside vendors). This includes Apple. You can assume they have tested their libraries, probably more than you could do. However, you should write tests that verify that your code communicates to 3rd party correctly. This can just be writing a spy that duplicates the interface of some 3rd party code and verifying your code calls the right functions in the correct order and handles any error cases.

As I mentioned above, unit tests should be very fast, and should be run after almost every change you make, certainly after every major change.

There are many articles on unit testing, so I won’t go into much detail. The main thing is to remember *“Arrange, Act, and Assert”*. This means a given test sets up all the preconditions for a certain scenario you’re testing (Arrange), executes the scenario (Act), and then verifies the after effects (Assert).

Another thing to keep in mind is that you want to test behavior and not implementation details. If you test implementation details, your tests become brittle, and every time you change your implementation (like when refactoring), your tests break.

### TDD
TDD, or Test-Driven Development can be useful, especially when used to flush out a good API and design for your module. It’s also great for tricky code that is heavily dependent on algorithms or other complex logic.

It can also be a great way to track what you need to do. Just write all the tests you need up front to verify functionality, then slowly complete them. Maybe comment out the ones you’re not working on yet. When they all pass, you might be done, assuming you didn’t miss any test cases. I’ve know some developers whose thoughts (and code) seem to be all over the place. TDD can help keep you focused and on track.

You can also use TDD for when you don’t know how to solve a problem. Create some tests for outputs you know are correct. These tests will fail until you figure out a solution. Then work on your solution until all your tests pass.

## Integration Testing
Whenever someone brings up UI tests or integrated (see definition above) tests, I always point them to [Integrated Tests Are A Scam](https://vimeo.com/80533536), a great talk by J.B. Rainsberger. In his talk, he explains how it is basically impossible for integrated, or end-to-end tests, to test all the possible combinations of state your software can be in. Any time you add new state, the possible combinations are exponential and it is impossible to keep up with all the tests you would need to write to cover all the situations.

Instead, you need to setup clear boundaries between the different parts of your software with clear communication protocols. Then all you need to test is that one part interacts correctly with the boundaries of any other parts it interacts with. This is completely doable.

This includes UI code. Since UI code is dumb (see above), you just need to verify a few things and that can be done in a unit test. For example, if there is a button, you just need to verify that 1) the button exists, and 2) the button is setup to send a certain message when it is pressed. You don’t actually need to press the button. For SwiftUI, this can be done with such tools such as [ViewInspector](https://github.com/nalexn/ViewInspector).

## UI Testing
I’ve already written about why I think UI tests are a bad idea. However, if you really must test the UI (management never listens to me), I recommend a few things.

First, If you just want to test that your design is pixel perfect, use snapshot tests. See [pointfreeco/swift-snapshot-testing: 📸 Delightful Swift snapshot testing.](https://github.com/pointfreeco/swift-snapshot-testing).

Frankly, I think this can be covered by manual tests. If a QA engineer doesn’t see any problems in the visual look of a UI, then it’s probably not a problem to worry too much about.

Second, have your QA engineers write the UI tests. Your developers will thank you for not having them spend hours writing the tests and even more time re-writing and debugging every time they break. Besides, the developers are too close to the code to write good UI tests. They don’t have the perspective of a user, which a QA engineer should have.

By the way, your QA engineers are engineers. They should know how to write code. If not, you might want to re-consider your hiring criteria for QA engineers.

## Acceptance Testing
I hate UI tests with a passion because I once spent an entire month trying to get them to work reliably (with no help from anybody else, including management). They took an hour to run and would usually fail, but if you did the same actions manually, they would work fine. As a result of this, I’ve always wanted to try Acceptance tests as an alternative to UI tests. So far I haven’t had the chance or convinced anybody to investigate it. Here’s a definition:

> **Acceptance testing** is a type of testing performed to determine whether a system or software satisfies the specified business requirements for delivery. Also known as user acceptance testing, it is typically conducted by real end-users or clients, or by testers acting on their behalf.

Basically, they are tests written at the level of business requirements and are supposed to be written by business analysts, product managers and the like. They are written at a very high level, almost English. [Fitnesse](https://fitnesse.org/) is one tool that lets you write acceptance tests using a wiki.

## Manual Testing
When I first started my career, manual testing was the norm. Lately, the trend is away from manual testing and towards automated testing, with some organizations not even having a QA department.

In my opinion, you need both. We have already talked about automated tests and their benefit is obvious (who doesn’t want to automate everything?), but you still need manual testing. Someone needs to actually use the software to verify that it is easy to use and satisfies its purpose. Automated tests can’t do this. Everybody should be using the software (dogfooding), so happy paths should be well tested just by use. Again, no need for UI tests testing happy paths. Unit and integration tests should test error handling and unusual conditions.

## Quality Assurance (QA)
In my experience, organizations do not treat their QA people well. It’s treated as an afterthought, and they hire people they can pay as low as possible. They are often treated interior to developers, with less pay, less benefits, and less working conditions.

As I mentioned above, QA people should be QA engineers. They should be hired as skilled people who understand code and can design and develop their own testing tools to test your software (such as UI tests). These kind of people with the right mentality and desire to test are worth their weight in gold.

Developers are too close to the code to be objective about testing it. You need an independent group that can provide another perspective on your code quality.

## Conclusion
Those are my thoughts. In my experience, organizations put way too much faith in UI testing and end-to-end tests. They get the testing pyramid turned upside down. They also underestimate the value of highly-skilled QA engineers and manual testing.

I’m available for consulting, so if you need help addressing these issues, I can provide suggestions and guidance.

