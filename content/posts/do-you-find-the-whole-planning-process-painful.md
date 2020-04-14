---
title: 'Do You Find The Whole Planning Process Painful?'
date: Wed, 09 Nov 2016 22:51:27 +0000
draft: false
tags: ['Software Development']
---

A [thread](https://www.reddit.com/r/AskProgramming/comments/5bsrx1/do_you_find_the_whole_planning_process_painful/ "thread") in the AskPrograming forum on reddit started with this question and I thought I would share my thoughts on planning and up-front design. 

Basically, the originator of the thread is expressing his dislike of planning before coding, but thinks it’s a good idea because they’ve either been told that or seen others do it. They also expresses dislike for planning tools and wonders if there’s a better way. Basically, they want to know how do you plan and how do you break up a project into tasks that you need to do. 

First of all, I agree that one big, cover all contingencies plan is not the way to go. We are in an Agile world and we value working code over extensive documentation. Practices, like iterations and TDD, help us to change course to handle changes in requirements. I also agree that you don’t need complex software tools to do planning. 

However, in my experience, most companies practicing Agile give short shrift to planning and simply decide what tasks from the backlog will go into the next iteration without too much thought. I’ve come to realize that if we just take that task from the backlog and think about it a little more, we experience some great benefits. 

Many times, the task is too big or not detailed enough. Split it up into smaller tasks. This requires thinking about the individual steps that make up the task and in what order the steps need to be done. This is very worthwile. The individual steps become new tasks and because of their smaller size they become easier to estimate how long it will take to accomplish them. You can also see the dependencies between the steps which will allow you to possibly assign them to people on the team for parallel development and move others to later iterations based on their priority. Use a simple tool like [Trello](https://www.trello.com) to organize all the tasks and iterations. 

I like to finish each iteration with working software that provides some kind of useful functionality for the customer. When you’re first starting a project this can be difficult, but if you break up the tasks properly it can be done, and at the end of each iteration, your customer can actually start using the software and provide feedback further directing your future iterations. 

The other thing I’ve come to realize is that a little up-front design/architecture is needed. It provides structure, and discipline to your project. Developers will know where things belong and how to keep things separate. Pick an architecture that provides more guidance than Apple’s code examples. They’re popping up everywhere now because I think everybody is coming to the same conclusion. Pick one like [VIPER](https://www.objc.io/issues/13-architecture/viper/), [MVVM](https://www.objc.io/issues/13-architecture/mvvm/), [Clean Swift](https://www.clean-swift.com), or some other one. I prefer Clean Swift which is based on the [Clean Architecture](https://8thlight.com/blog/uncle-bob/2012/08/13/the-clean-architecture.html) from Uncle Bob ([here’s](https://www.youtube.com/watch?v=Nsjsiz2A9mg) a great video that helped me understand its benefits). These architectures can be overkill if you’re just on your own, but when you’re on a team, I think they are a necessity for the long term health and viability of your product. 

Having an architecture and practicing TDD will allow your development to proceed at a sustainable good pace. It may slow you down in the beginning, but I believe, and I think Uncle Bob said this, “You have do go slow to go fast.” (He must have been channeling Yogi Berra when he said that.) Software development is a marathon. If you start off sprinting or coding as fast as you can, you’re not going to be doing too well when you’re some time period down the road. That time period can be surprisingly small. 

Planning doesn’t have to be painful. A little planning can be very beneficial and fun. I enjoy the direction and order an architecture gives to my project and always having working software that I can demonstrate.
