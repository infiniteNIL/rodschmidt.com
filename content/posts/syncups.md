---
title: "Pointfree's SyncUps App: A Great Example Architecture for a SwiftUI App"
date: 2024-05-11T10:41:54-06:00
draft: false
tags: ['Architecture', 'Apple Platforms', 'Swift', 'SwiftUI']
---
In comments to my last article on [The Composable Architecture](/posts/composable-architecture-experience) readers have asked about examples of how I think an app should be written. In the article, I did mention my previous posts, which have examples for Clean Swift and MVVM, but those were written for UIKit. SwiftUI didn’t exist at the time.

So, this article will talk about what I think is a great example of architecture for a SwiftUI app. Pointfree’s own [SyncUps](https://github.com/pointfreeco/syncups) app. The example actually has 2 apps, one showing tree-based navigation, and one showing stack-based navigation. For this article, I will just focus on the stack-based example.

Despite my criticisms of TCA in my previous article, I’m a big fan of Pointfree. Besides TCA, they have released a lot of great libraries that are very useful to improve SwiftUI apps. These libraries are used to great effect in SyncUps.

## SyncUps Architecture

SyncUps basically has what I would call a MVVM+ architecture. Each view has a model, which the view forwards user events to, and the view gets data from the model to display. The view model also handles interacting with the data model and outside services, such as `DataManager` (persistence) and `SpeechClient`, a wrapper around Apple’s Speech framework that provides speech recognition functionality. The additional services are what I call the ‘+’ in MVVM+. You don’t want to cram all the stuff into the view model and you may need to reuse that functionality across view models. Other examples would be interacting with a REST API, which is probably the most common need.

Here are the views and associated view models in SyncUps:
| View                 | View Model          |
|----------------------|---------------------|
| `AppView`            | `AppModel`          |
| `SyncUpDetailView`   | `SyncUpDetailModel` |
| `SyncUpFormView`     | `SyncUpFormModel`   |
| `SyncUpsListModel`   | `SyncUpsList`       |
| `RecordMeetingModel` | `RecordMeetingView` |

## Data Model

In addition to view models, there are traditional data models, with `SyncUp` being the main one:
| Data Model |
|------------|
| `SyncUp`   |
| `Attendee` |
| `Meeting`  |
| `Theme`    |

The [swift-tagged](http://github.com/pointfreeco/swift-tagged) library is used to provide more type checking for model identifiers. This prevents bugs such as mistakenly passing an Attendee ID to a function that expects a SyncUp ID.

[swift-identified-collections](http://github.com/pointfreeco/swift-identified-collections) provides an “identified” array. With this you can, for example, access an attendee of a SyncUp by its ID, rather than its index. This is a better way to access an attendee, as its index may change, such as when an attendee is deleted.

## Dependencies
I mentioned services above. Pointfree likes to call these dependencies and they have a library, [swift-dependencies](http://github.com/pointfreeco/swift-dependencies), to manage them. For each external dependency, you wrap it in what is called a client, providing just the API that your app needs. A client can provide a protocol for talking to an outside service, or just a bunch of closures. These dependencies can also be replaced at run-time. This is especially useful for tests and previews, allowing you to mock the dependencies and control them.

It also provides easy access to services with the `@Dependency` macro, which works a lot like SwiftUI’s `@EnvironmentObject` and frees you from having to pass your dependencies around everywhere.

## Navigation
SyncUps uses `NavigationStack` introduced in iOS 16. This along with the `.navigationDestination` view modifier, lets you control your app’s navigation by simply having a stack of some state. This stack represents a path to a view in your app. So to navigate somewhere, you just add or remove state to/from the path.

Pointfree provides the [swiftui-navigation](http://github.com/pointfreeco/swiftui-navigation) library and the [swift-case-paths](https://github.com/pointfreeco/swift-case-paths) library that lets the app do this in a very elegant way. You can represent every destination in your app with a simple enum with associated data. No need to model it with complicated structs or multiple booleans. Just put a bunch of enums on the path and your app will navigate there. Of course, you have to have the appropriate `.navigationDestination` and `NavigationLinks` in place. This also makes it incredibly easy to implement deep linking.

## Testing
SyncUps has unit tests that tests each of the view models and exercises all their features. They use the [xctest-dynamic-overlay](https://github.com/pointfreeco/xctest-dynamic-overlay) library to help provide test helpers in your main library that you can use in your tests. These helpers provide such things as mock implementations of your dependencies/clients. See the library repo for a more in-depth description of what it provides.

SyncUps also has some UI tests. This was surprising to me as I don’t recommend writing UI tests. Then in the comments at the top of `SyncUpsListUITests` you see

```swift
// This test case demonstrates how one can write UI tests using the swift-dependencies library. We
// do not really recommend writing UI tests in general as they are slow and flakey, but if you must
// then this shows how.
```

I very much agree. I did run the tests and they worked well. I think this has a lot to do with the fact that they use a different root view of the application when running UI tests (`UITestingView` instead of `AppView`). In that view they mock a lot of dependencies. This helps in the stability of the test as they are just testing the UI and not the integration of everything.

One thing the app doesn’t do is demonstrate snapshot testing. Pointfree provides [swift-snapshot-testing](https://github.com/pointfreeco/swift-snapshot-testing) to really help with this, making it trivial to use snapshots to test things, whether it be UI or complicated output from a function (maybe a JSON encoding). You can use snapshots to generate screen shots for the App Store as well.

## Things I’d Do Differently
### Put View Code in Separate Files
I like to keep my view code completely separate from my models. SyncUps puts the view code in the same file. I like to do this because it helps enforce the notion that the view code shouldn’t be doing any business logic. It just display things and tells the view model something happened.

### Organize by Scene
I like to organize my code by each screen in the app, or “scene” as I like to call it. This could also be by feature or use case. So for each “scene”, I have a folder. In that folder is the code that implements that scene, minus data models and services, which are in their own folders since they are used in multiple places.

### Coordinators?
Another thing you might consider doing is using the coordinator pattern.  See [How to Use the Coordinator Pattern in SwiftUI](https://quickbirdstudios.com/blog/coordinator-pattern-in-swiftui/). This allows you to separate navigation logic from your views and makes it easier to use a view in many places throughout your app. If your view has hard-coded navigation to other views, this isn’t possible.

## M\*V?

The Composable Architecture, MVVM, MVC, MVP, etc. are really just UI design patterns. They are not the complete architecture of your app. When it comes down to it, a UI design pattern is really just a way of separating your UI from your business logic. In other words, you have a bunch of code that models your domain (i.e. models), and you have a bunch of code that implements the user interface for the user to interact with that domain. It is important to separate these because it is not easy to test UI and your UI is the thing that changes the most. A view model, presenter, interactor, or whatever is just another model that encodes how your business logic interacts with the UI. By separating out this code, you can test that behavior as well. The view just becomes a fairly dumb thing that just displays data and tells your models the user did something. Ideally, the view becomes so simple that there isn’t much need to test it. All the important logic is outside the view and easily testable.
