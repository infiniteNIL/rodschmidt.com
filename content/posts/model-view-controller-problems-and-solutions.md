---
title: 'Model View Controller: Problems and Solutions'
date: Tue, 22 Nov 2016 21:41:19 +0000
draft: false
tags: ['Apple Platforms', 'Software Development']
---

_Model View Controller_ or _MVC_ is the application architecture used by default for applications on all Apple platforms. Most of the tools, frameworks, and docs from Apple all talk about it and support it. In MVC, objects are assigned 1 of 3 roles:

* **Model** - objects that encapsulate and manage the data the application works with (this includes persistence). The data typically represents things in the real world like an employee, hardware part, or a picture that is being drawn. More abstract things can also be part of a model such as a hiring process. When data in the model changes it notifies a controller object, typically via delegation or notifications. Model objects should have no knowledge of the user interface and be reusable in other applications in similar problem domains or on other devices.

* **View** - user interface objects that displays the model’s data and respond to user interface events such as taps, swipes, mouse clicks, etc. The view has no knowledge of the model. It simply display what a controller tells it to. Events are likewise just relayed to the controller.

* **Controller** - The object that coordinates the model and the view. It handles user interface events and translates them into actions on the model. When the model changes it tells the view to update itself with new data it obtained from the model.

Here’s a nice picture from Apple’s documentation that nicely depicts the above description: 

{{<figure src="/images/model_view_controller_2x.png" alt="Model View Controller Diagram" width="768">}}

\
For a more detailed description of MVC, I highly recommend you read [Apple’s documentation on MVC](https://developer.apple.com/library/etc/redirect/xcode/content/1189/documentation/General/Conceptual/DevPedia-CocoaCore/MVC.html "Apple documentation on MVC").

Problems with MVC
-----------------

In the iOS development world, MVC is sometimes jokingly called “Massive View Controller” for the tendency for iOS view controllers to end up with lots of code and do all kinds of things not related to just mediating between the view and the model, such as load data from databases, and access the network, among others. 

iOS apps have a lot of responsibilities. Models (represents data, persistence) and views (display data, UI events) are fairy well defined, so when something doesn’t belong in the model or the view, it ends up in the only other place there is, the controller. Thus they tend to become very big. 

iOS, with UIViewController, and increasingly macOS, with NSViewController, are prone to this because view controllers are so closely linked to the view. In fact, they are so closely linked that they should really be considered a part of the view. 

Exacerbating the problem, there is no class, interface, or other API in the iOS SDK that supports writing a non-view controller. Cocoa, on macOS, has some controllers such as NSArrayController. Developers naturally gravitate to the only thing they have available, UIViewController. 

So, what’s the problem with a lots of code in the controller, you might ask? Lots of code doing different things means the controller has lots of responsibilities making it prone to being changed a lot when requirements change, hard to reuse, and hard to test. Testing is difficult because of all the dependencies the controller has. It is very difficult to isolate the controller for a test or recreate all the dependencies in such a way that you can setup the conditions for a test.

Doing MVC Right
---------------

I believe MVC can be used properly in an iOS application, but it requires discipline. I think it boils down to this: One must follow the [Single Responsibility Principle](https://en.wikipedia.org/wiki/Single_responsibility_principle), especially in your controllers. If you do that, you have a chance. The problem is there is nothing in the Apple’s tools or SDKs that help with that discipline. Xcode won’t help you. Cocoa won’t help you. The compiler won’t help you. 

That’s where architecture comes in. With a little more architecture you can have some help in being disciplined. Your controllers will become smaller, easier to change, easier to reuse, and easier to test. With the right implementation of your architecture, even the compiler will help you. Next time, we’ll start exploring those solutions, with one solution in particular: The Clean Architecture.
