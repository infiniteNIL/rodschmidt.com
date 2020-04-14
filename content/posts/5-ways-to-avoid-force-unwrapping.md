---
title: '5 Ways to Avoid Force Unwrapping'
date: Wed, 16 Nov 2016 18:02:53 +0000
draft: false
tags: ['Swift']
---

Someone on the reddit iOS Programming group asked [“What are the mistakes generally done by iOS developers while coding in Swift?”](https://www.reddit.com/r/iOSProgramming/comments/5bm9j7/what_are_the_mistakes_generally_done_by_ios/)A lot of comments were about using force unwrapping on optionals. Comments such as:

* Force unwrapping everything
* Using pyramids of if-let as opposed to guard statements
* `var firstName: String!` ugh
* Overuse of force unwrapped optionals. I cringe when I see that especially when a simple one-liner guard statement would make the code a ton safer.

So, I thought I would address these concerns and list 5 ways you can avoid force unwrapping.

1\. Guard
---------

To set this up, let’s say we have a function called `fullName` that takes a first name, optional middle name, and last name and returns a string for the full name. 

So `fullName(first: "Rod", middle: nil, last: "Schmidt")` results in “Rod Schmidt” 

With force unwrapping this might look like this:

```swift
func fullName(first: String, middle: String?, last: String) -> String? {
   if middle != nil {
       return  first + “ “ + middle! + “ “ + last 
   } else {
       return first + “ “ + last
   }
} 
```

One way to avoid the force unwrapping is with a guard, like this:

```swift
func fullName(first: String, middle: String?, last: String) -> String? {
    guard let middle = middle else { return first + " " + last }
    return  first + “ “ + middle + “ “ + last 
} 
```

2\. If Let
----------

Taking the same example above, we could use an if-let statement instead:

```swift
func fullName(first: String, middle: String?, last: String) -> String? {
    if let middle = middle {
        return  first + “ “ + middle + “ “ + last 
    } else {
        return first + " " + last
    }
} 
```

3\. Let With 1st Assignment
---------------------------

This one is not widely known or used. Did you know you can declare a let without assigning to it? As long as you assign something to it before it is used, you can do this. Here’s a rather contrived derivation of our example above that adds a title to the name based on whether or not the first and middle name are provided:

```swift
func fullName(first: String?, middle: String?, last: String) -> String? {
    let title: String
    
    if let first = first, let middle = middle {
        title = "M."
        return title + " " + first + " " + middle + " " + last
    } else {
        title = "X."
        return  title + " " + last 
    }
} 
```

Also notice the multiple lets in the if-let statement instead of nesting them.

4\. Nil Coalescing Operator (??)
--------------------------------

I like this one a lot. It’s a way to provide a default value if an optional variable is nil. Taking our first example above, we could do something like this:

```swift
func fullName(first: String, middle: String?, last: String) -> String? {
   return  first + " " + (middle ?? "") + " " + last 
} 
```

We just concatenate an empty string if middle is nil.

5\. Optional Chaining
---------------------

This comes into play quite often when doing UIKit stuff. Say you have a delegate that you know will be set by a XIB or storyboard when it’s loaded. So you may have a delegate declared like this: `var weak delegate: MyDelegate!` 

When a button is pressed, you need to tell the delegate. Instead of using the implicitly forced unwrapping you can do this: `delegate?.buttonWasPressed(self)` 

Then delegate will only be called if it is not nil. Normally, your delegate is quite possibly nil and you always want to do this, but I’ve seen code like this:

```swift
if delegate != nil {
   delegate!.buttonWasPressed(self)
} 
```

Totally unnecessary and verbose. Just use optional chaining.