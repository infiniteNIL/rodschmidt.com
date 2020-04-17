---
title: 'The Clean Architecture: An Introduction'
date: Fri, 09 Dec 2016 00:33:44 +0000
draft: false
tags: ['Clean Architecture', 'Software Development', 'Testing']
---

In the last [post]({{<ref "moving-towards-the-clean-architecture-for-apple-development">}}), I talked about various architectures used as alternatives to MVC, in a attempt to solve MVCs problems, such as Massive View Controller. 

In this post, I would like to introduce you to another architecture, which seems to me to be the best starting point for your app’s architecture: the Clean Architecture. 

I think I first ran into the Clean Architecture in one of [Uncle Bob’s presentations on YouTube](https://www.youtube.com/watch?v=WpkDN78P884). At first, I kind of wrote it off as a typical Java over-engineered design pattern. It wasn’t until earlier this year, probably years later after my initial discovery, that I ran into it again. I was looking for a better app architecture after an experience working for a large company with a large iOS team (with about 11 developers) and all the issues they had getting the app to work and test it. I kind of became obsessed with figuring out how to make an app easier to test. I searched the internet for app architectures and studied them. It took me awhile to get my head around the Clean Architecture, but once I did, I decided it would be my starting point for an app architecture. 

Uncle Bob also has a [blog post](https://8thlight.com/blog/uncle-bob/2012/08/13/the-clean-architecture.html) on the Clean Architecture.

Origins and Resurgence
----------------------

The roots of the Clean Architecture go back to the 1993, when the book [Object-Oriented Software Engineering: An Use-Case Driven Approach](http://amzn.to/2g74J3X) by Ivar Jacobson was published. In it, Mr. Jacobson proposed a design approach where the use case was the central concept an application design was built around. A use case is very similar to what we now call a user story. It’s basically a function the user can do with the app, such as create a new user in a payroll app, for example. 

According to Uncle Bob, the industry was just starting to learn that this was a good way to design applications, when in the mid to late 90s, the web arrived and threw everything into disarray. We had to figure out how to write web applications and we stopped thinking about how we did it for desktop applications. 

Eventually we started thinking about architecture again, and MVC was adapted to web applications. That is probably how most developers were introduced to MVC. Maybe more recently when they started doing iOS and mobile development. Uncle Bob says MVC is not a architecture, but a design pattern. Originally, it was designed to control one single thing on the screen like a button. Every button, text field, etc., would have its own controller, view, and model. It was then adopted as an application architecture and this is the cause of its problems. 

Because of these problems, everybody is thinking about architecture again and we’re seeing an explosion of app architectures. People are looking for solutions to their problems such as testing and writing code that is easy to maintain. 

The Clean Architecture is very similar to these other architectures that you may have heard of:

* **[DCI](http://www.artima.com/articles/dci_vision.html)** (Data, Context, and Interaction) created by Trygve Reekskaug and James O. Coplien. Mr Reenskaug is the original creator of MVC.

* **[The Hexagonal Architecture](http://alistair.cockburn.us/Hexagonal+architecture)** created by Alistair Cockburn. Also known as **Ports and Adapters**

* **[The Onion Architecture](http://jeffreypalermo.com/blog/the-onion-architecture-part-1/)** by Jeffrey Palermo.

How it Works
------------

I like to think about the Clean Architecture as a way of creating an engine for your application, much like you would create a game engine. You have some code tucked away in some libraries that is independent of the platform it will run on, or its environment. That’s really what it’s about. 

Here’s a diagram of the Clean Architecture: 

{{<img-center src="/images/CleanArchitecture-8b00a9d7e2543fa9ca76b81b05066629.jpg" title="from 8th Light">}}

The engine, or core of your app, are the 3 circles (yellow, red, and green) in the middle. They contain the entities, use cases, interactors, and other objects of your engine. They know nothing of UIKit, Core Data, Realm, HTTP, the web, or any other delivery mechanism, as Uncle Bob calls them. 

The outer blue circle contain the external things your app needs to talk to, such as a delivery mechanism (i.e. the UI, UIKit, A web browser, etc). It also contains other things that your app needs to talk to to relate to the outside world, such as databases, networks, or even a sensor somewhere. Examples would be Core Data, a REST API, or a bluetooth sensor. These are not part of your engine or app core, and your app core will not know anything about them. It will know about some protocols it can use to talk to them. 

The Clean Architecture has the following components:

* **Interactors** - Represent the use cases and are the central objects in the Clean Architecture. You will have an interactor for every use case and they will be named after the use case. It is responsible for executing the use case by talking to entities and gateways. All the interactors together contain your application specific business logic or rules.

* **Entities** - Represent the business objects and contain business logic that could be used in multiple applications. Unlike in frameworks, they do not inherit from NSManagedObject or ActiveRecord. They are just plain simple objects, probably just structs in Swift. They do not know about the database or your network layer.

* **Presenters** - Contain your presentation logic. It is called by an interactor (through a boundary or protocol) to present something to the delivery mechanism. It does this by creating a ViewModel and passing that to the view (or view controller). Again, the presenter knows nothing of UIKit, it simply talks to an interface.

* **ViewModels** - In the Clean Architecture, the view model simply contains simple data (strings, ints, boolean, etc.) the view needs to display. It just a simple struct that contains all the data the view needs (which was prepared or formatted by the presenter). The view is very dumb. All it does is take the data and place it where it needs to go.

* **Boundaries** - You will hear Uncle Bob talk about Boundaries. They are simply just interfaces or protocols in Swift that all communication must go through to talk to another layer (or ring) in the architecture.

* **Gateways** - A gateway is simply another kind of boundary to talk to the outside world like a database, network, service, or other device that your core might need to talk to. Uncle Bob uses the term Gateway, but you and I are probably more likely to give then specific names like DatabaseStore or NetworkAPIClient for example. Again, these would be protocol names or abstract classes.

### Relationships and Data Flow

Here’s another diagram I created from some other diagrams Uncle Bob had in his talk: 

{{<img-center src="/images/Clean-Arch.-Data-Flow.png" title="Clean Architecture data flow" width="768">}}

Let’s say we have a create user use case. Here’s how everything works:

1. The user kicks things off by tapping on a new user button. Let’s say that brings up a form that the user fills out and then they tap the done button.

2. The view controller action connected to the button gets called. It creates a _RequestModel_, fills it with the data from the form, and passes it to a method on the boundary (or protocol) that our CreateUser interactor implements. The _RequestModel_ is simply a struct that contains all the data needed by the use case. Do not include any UIKIt types in the request model. Ideally, just Swift types. Maybe some Foundation types depending on how independent you want to be from Apple’s APIs.

3. The interactor takes the data in the RequestModel and calls a method on the Entity Gateway Interface (or protocol) implemented by an Entity Gateway Implementation. Probably a method called something like createUser() with parameter(s) for all the data needed to create a user.

4. The Entity Gateway Implementation talks to the Database API (maybe Core Data) and creates a user in the database or whatever our storage mechanism is. It then takes the result of creating that user and creates a User Entity and passes that back to the interactor. The User entity is simply a struct that contains information about the user. It does not contain any storage mechanism specific information.

5. The interactor takes the user entity and creates a _ResponseModel_, filling it with the data from the entity. The _ResponseModel_ is very similar to a RequestModel, except it is for returning a response through the boundary. The Interactor then calls a method on the boundary (or protocol) implemented by the presenter, passing it the response model.

6. The presenter takes the response model and creates a ViewModel filling it with data from the response model. It then calls a method on the view controller, passing it the view model.

7. The view controller then uses the view model to populate the view. In this case the view controller might simply present an alert that says the user was created. It might also add the the user’s name to a list that is displaying and refresh the list.

Testing
-------

One of the benefits and main goals of the Clean Architecture is testability. Uncle Bob advocates doing Test-Driven Development (TDD), saying that writing tests after the fact are a waste of time. The tests guide your design. If the tests are hard to write then something might be wrong with your design. The tests let your refactor very quickly and tell you when you are done. 

The Clean Architecture makes things easy to test with its use of boundaries. Anywhere there is a boundary (or protocol), you can substitute another object that implements the protocol. Your delivery mechanism can just be your tests. Your Entity Gateway Implementation can just return some stubbed data. This makes it very easy to test things in isolation by replacing all of an object’s dependencies with stubs or mocks. This also makes the tests very fast. 

The views are really dumb and don’t really need to be tested. You can write some tests just to make sure that they call the right boundaries methods when they should. You just replace your interactor with a spy that records when a method gets called. Your tests just verify the methods are called. 

Uncle Bob says you don’t need to test the views or UI. I would tend to agree with him, but I’m not sure you can completely get away with that with management. It’s easy enough to add the tests I described above. I would leave testing of the rest of the UI to the acceptance tests. Remember, you want your unit tests to run very fast. You will be using them constantly. UI testing (by that I mean user-based end-to-end black box tests) will always be be slow.

### Acceptance Testing

J.B. Rainsberger is a consultant that helps companies solve their architectural and testing issues. I highly recommend his [blog](http://blog.thecodewhisperer.com/series#integrated-tests-are-a-scam). In a presentation called “[Integrated Tests are Scam](https://vimeo.com/80533536)”, he talks about how it is impossible to write integrated tests that cover everything. Your tests just get slower and slower as your app grows. There are just too many code paths. All you really need to test is the boundaries. Hence the need for a good architecture like the Clean Architecture. 

Have your developers write unit tests for everything (maybe even a little view testing with everything else stubbed out). They can even write some unit tests that test the integration between the different layers (i.e. test the boundaries). Leave the end-to-end testing, or acceptance tests to your QA engineers. Their job is to determine if the software is acceptable by the customer. They just need to write acceptance tests that test each use case from a user’s viewpoint. Uncle Bob was involved in the creating of an acceptance test framework called [Fitnesse](http://fitnesse.org).

Summary
-------

Uncle Bob says a good architecture maximizes the number of decisions not made. The Clean Architecture does that by providing clear boundaries between different parts of your app in such a way that your app doesn’t care how things behind the boundaries are implemented. This gives you tremendous flexibility and independence. You can plugin different drivers of your app (i.e. UIs or tests) whenever you want. Same thing with the other external dependencies of your app (i.e. database, network, etc.) 

This article presented a lot of information, and it will probably talk multiple readings and studying of the articles and videos I linked to to fully wrap your head around it. I believe you will find it worthwhile.

Next Time
---------

Next time, we’ll get practical. I plan to take a classic or common Cocoa example that uses MVC and convert that to the Clean Architecture.
