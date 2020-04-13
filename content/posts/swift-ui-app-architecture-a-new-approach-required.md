---
title: 'Swift UI & App Architecture: A New Approach Required'
date: Sun, 04 Aug 2019 17:47:09 +0000
draft: false
tags: ['Apple Platforms', 'Clean Architecture', 'Software Development', 'Swift', 'SwiftUI']
---

With SwiftUI, everything has changed. As Brent Simmons said, it’s the end of the NeXT era and the beginning of the Swift era.

With the coming of SwiftUI, most of our app architectures are no longer valid (or at least parts of them). They need to be adjusted. MVC is certainly out. In fact, even controllers seem to be out. From Apple’s content, it seems all we have is models and views, and that’s true. Bindings have seemingly replaced controllers.

At my place of work, we have been using an architecture called MVP+, which I like to think of as a cross between Model-View-Presenter and Clean Architecture. I have been working with SwiftUI on some company projects and my own, and the architecture doesn’t fit anymore.

So, is it time to come up with a new architecture for SwiftUI. Just like with MVC, there is a natural inclination or easy path to just put everything in the view. This will of course just lead to _Massive Views_.

With some views having actions (such as Buttons), there is a tendency to scatter your event handling and state changes throughout the view. We’d like to have it all nicely organized and handled in one place.

And with bindings, there is a tendency to just adapt your model objects to adopt the new protocols, but that makes your app specific code dependent on SwiftUI or Combine, and we don’t want that. In keeping with Clean Architecture, we want our app specific logic to be completely independent. Your models shouldn’t be dependent on Combine or SwiftUI.

I will be coming up with my own architecture, and in future posts, I’ll talk about it. I’m thinking there will be influences from Elm, Clean Architecture, MVP+, MVVM, and functional programming.