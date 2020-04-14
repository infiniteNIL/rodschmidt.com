---
title: 'Swift Robot Arm'
date: Tue, 11 Oct 2016 22:36:54 +0000
draft: false
tags: ['Robots', 'Swift']
---

In a previous [post]({{<ref "elixir-robots.md">}}), I wrote about controlling my OWI robot arm with Elixir. Well, I decided to port that to Swift! 

In that [post]({{<ref "elixir-robots.md">}}), there’s a link to some code that does it in Objective-C with IOKit. I first tried to just do a straight port and use IOKit in Swift. That didn’t work to well. The IOKit API is an old Core Foundation library and even has some old [COM](https://en.wikipedia.org/wiki/Component_Object_Model) style APIs. There’s some complex macros as well that do not even get transferred to Swift, so you have to find their definition and and put them in a Swift constant or function. I couldn’t even get it to compile. 

So, I looked at the low-level libusb C code that the Elixir project used and decided to just port that to Swift. It was tricky getting all the C pointer to pointers to variable sized structures working with Swift’s UnsafeMutablePointers and such but once I satisfied the compiler it pretty much worked. 

I then refactored things and created a OWIRobotArm class with a nice API, pretty similar to the Elixir API. I’ve put the code up in a Github repository called SwiftArm. You can find it [here](https://github.com/infiniteNIL/SwiftArm "SwiftArm").
