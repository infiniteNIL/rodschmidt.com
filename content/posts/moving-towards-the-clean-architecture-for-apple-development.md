---
title: 'Moving Towards The Clean Architecture for Apple Development'
date: Wed, 30 Nov 2016 18:09:30 +0000
draft: false
tags: ['Software Development']
---

In the last [post](https://rodschmidt.com/model-view-controller-problems-and-solutions), I talked about MVC, its problems, and how it could be done right. Various architectures have emerged to try to address the deficiencies of MVC. Before I talk about the Clean architecture, I like to talk about some of them.

Alternative Architectures to MVC
--------------------------------

Up front, I don’t have a lot of experience with these, but I have studied them and I believe they are a step in the right direction. They all introduce another object to take over some of the responsibilities from the controller, but I don’t think they go far enough because the new object typically has too many responsibilities as well and tends to grow just as the controller in MVC does. They do spread the load though, reducing the issues with MVC and increasing testability.

### Model View Presenter (MVP)

MVP treats UIViewController as part of the view (as it should), and introduces a new object, called the Presenter. \[caption id="attachment\_287" align="alignnone" width="312"\]![from Wikipedia](https://rodschmidt.com/wp-content/uploads/2016/11/Model_View_Presenter_GUI_Design_Pattern.png) from Wikipedia\[/caption\] Unlike the view controller, the presenter knows nothing of UIKit. It talks to the view through a protocol the view implements. The presenter handles presentation logic and is responsible for updating the view and responding to view events and translating them into model changes. The view talks to the presenter through a protocol as well. The protocols make it easy to mock the view or the presenter making it easy to test the presenter or the view in isolation. I think MVP was popular in the Microsoft world and their tools supported it. I’m not sure that is still the case. I think it has been supplanted by MVVM.

### Model View ViewModel (MVVM)

MVVM treats the view controller as part of the view as well, and introduces a new object, called the ViewModel. \[caption id="attachment\_288" align="alignnone" width="660"\]![from Wikipedia](https://rodschmidt.com/wp-content/uploads/2016/11/MVVMPattern.png) from Wikipedia\[/caption\] MVVM is pretty similar to MVP. Think of the ViewModel as a UIKit independent representation of the view and its state. MVVM typically introduces some kind of binding between the View and the ViewModel with some kind of functional reactive programming library such as RxSwift or ReactiveCocoa. The bindings cause UI events in the View to invoke changes on the ViewModel, in turn invoking changes in the Model. The ViewModel updates itself with the updated Model and the bindings cause the view to be updated. Communication between the View and the ViewModel is again done through protocols making it easy to mock and isolate things for easy testing. MVVM is becoming quite popular in the iOS world and is closely linked to Reactive programming. It is also sometimes called “Massive View Model” because it can suffer from the same problem as MVC.

### MVC-X

Developers are coming up with alternatives to MVC all the time. Two mentioned by some prominent people are MVC-N and MVC-S. MVC-S, or Model View Controller Store, was put forth in [iOS Programming: The Big Nerd Ranch Guide](http://rads.stackoverflow.com/amzn/click/B007OWBAB0 "iOS Programming: The Big Nerd Ranch Guide"). It basically adds a Store object into the mix that handles storing and fetching data either from a database and/or the network. All you’re doing here is moving that logic from the controller or model into another object. Again, its a step in the right direction, but not enough. Marcus Zarra, of Core Data fame, made a presentation called [MVC-N: Isolating networks calls from View Controllers](https://realm.io/news/slug-marcus-zarra-exploring-mvcn-swift/), which also moves network calls out of the controller or model into a separate object called the Network Controller.

### VIPER

VIPER is not like the other MVC based architectures, because it actually has a cool name ;). Actually, it’s because it is an implementation of the Clean Architecture that I will be delving into. VIPER stands for View-Interactor-Presenter-Entities-Router. The router is also known as the Wireframe. It was introduced in an article in [objc.io](www.objc.io) called [Architecting iOS Apps with VIPER](https://www.objc.io/issues/13-architecture/viper/). \[caption id="attachment\_286" align="alignnone" width="800"\]![from objc.io](https://rodschmidt.com/wp-content/uploads/2016/11/viper.png) from objc.io\[/caption\] VIPER treats the view controller as part of the view, but instead of introducing just one new object, it introduces 5: The Interactor, Presenter, Entities, a Router, and a Data Store. As a result there is a much better separation of concerns. I’m not going to say much more about VIPER since I will be delving into greater detail into another implementation of the Clean architecture in another post. The implementation I will talk about is [Clean Swift](http://clean-swift.com). I like it better because it has a unidirectional data flow, whereas VIPER has bidirectional data flow between the view and the presenter. It just made more sense to me and seemed more faithful to the goals and purpose behind Uncle Bob’s Clean Architecture.

Conclusion
----------

Let me conclude by saying that no architecture is a silver bullet. The architecture that work best for you depends on the app and the situation it’s being developed in. If it’s just you writing an app you might consider some of these to be overkill, especially VIPER. If you are in a team environment, building a large app, it just might be the thing you need to bring order to the chaos. Whatever your situation, remember that it boils down to one thing. Use the Single Responsibility Principle. Even with VIPER, you will need to break out new objects that implement some responsibility; an object for validating emails for example. If you stick to the Single Responsibility Principle and use protocols with dependency injection, you will end up with code that is easy to test and easy to change. Thanks for reading and stay tuned for the next article where I will start delving into the Clean Architecture. Also, please sign up for newsletter below if you’d like to receive new articles right to your inbox as well as other useful things in the future. I will not spam you.