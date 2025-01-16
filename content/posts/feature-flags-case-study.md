---
title: "Feature Flags Case Study"
date: 2025-01-16T10:34:43-07:00
draft: false
tags: ['Architecture', 'Software Development']
---

# Case Study: Improving the Efficiency of Software Development Teams Dependent on Another Team

In my last job, I was on a platform team. One of the responsibilities of a platform team is to support all the other software development teams in an organization by writing libraries and tools that can be used by the other teams.

One of the libraries the platform team provided was a library that managed feature flags for the iOS app that our company produced. The primary function of the library was to get the current value of a given feature flag. If you’re not familiar with feature flags see [Feature Flags 101: Use Cases, Benefits, and Best Practices](https://launchdarkly.com/blog/what-are-feature-flags/). Basically, they are used to turn off and on features of an application at runtime.

## Problem
Here’s the problem we had. The other teams created lots of feature flags. Almost everyday a new flag would need to be created. This meant updating the library to support the new flag. This led to lots of churn with new versions of the library being released almost everyday. Teams that needed that flag couldn’t move forward with their plans until they had the flag. Any other libraries that depended on the new flag had to be updated, built, and released as well. This led to more testing, increased build times and everything else that goes along with new releases.

## Solution
I recogonized this problem, and took it upon myself to solve it. The root of the problem was that in order to add a new feature flag, the platform library had to change, even though all the logic for managing feature flags didn’t change at all. All that changed was a flag was added, removed, or changed.

The answer was decentralized registration of feature flags. I added the ability for a feature flag to be registed at runtime, rather than basically hard-coded into the library. When the application started, one of the first things it did was register feature flags. Each team would define a feature flag provider in the application. When the application started up, it would ask each provider to register the flags a team needed.

There was some pushback to this, as some felt it was too complicated and not necessary. I might agree if there was just one team, but in a large project with multiple teams, steps like this are often necessary.

The new design was successful. The feature flag library became very stable, no longer changing almost everyday.

## I Can Help
If you have a problem like this where teams are depending on other teams, and it’s causing problems, I can help. I can also help with any software development problems you are struggling with.

Just click [here](https://www.stan.store/infinitenil) to setup a coaching call. We’ll discuss your problem and some possible solutions.
