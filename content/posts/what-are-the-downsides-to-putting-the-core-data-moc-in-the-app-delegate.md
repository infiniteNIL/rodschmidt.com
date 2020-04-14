---
title: 'What are the Downsides to Putting the Core Data MOC in the App Delegate?'
date: Thu, 25 Aug 2016 20:59:37 +0000
draft: false
tags: ['Apple Platforms', 'Software Development']
---

I saw this question on the [iOSProgramming](https://www.reddit.com/r/iOSProgramming) topic in reddit:

> I've seen a number of different ways to access the NSManagedObjectContext when working with Core Data, but I was wondering if there are any downsides to the way I've been doing it. Basically, I stick a computed variable in the AppDelegate, and grab it when I need it... Please let me know if you see any flaws... If not, feel free to use it!

The writer then shows the following code which is the computed variable:

```objective-c
static var myManagedObjectContext: NSManagedObjectContext {
   return (UIApplication.sharedApplication().delegate as!
           AppDelegate).managedObjectContext
} 
```

There are a number of downsides to this:

1. The app delegate is managing the Core Data stack. Classes should only have one responsibility. The app delegate is already responsible for managing application lifecycle. It shouldn’t be managing the Core Data stack as well.
2. You are completely dependent on Core Data and using it as your persistence method for your app. What if you decide to switch to Realm or something?
3. Any code you write that uses `myManagedObjectContext` will be dependent on the App Delegate.
4. Any tests you write will be dependent on the App Delegate and Core Data and will be hard to test and slow as a result.

This is basically the lazy developer’s way of doing things. Granted the Xcode templates generate the Core Data code in the app delegate and doing this makes it easy to get the MOC, but Apple’s example code and the Xcode templates are not always the best way to do things. They are frequently the opposite. In this post, let’s just focus on the 2nd issue, that of being dependent on Core Data and using it as your persistence method. Perhaps I will cover the other issues in future posts. 

First off, you’re probably thinking, why would I use anything other than Core Data? What’s the problem with being dependent on Core Data? I hear discussions all the time about people not liking Core Data and wanting to switch to Realm, but this can even be a problem if you never plan on using anything other than Core Data. 

Here’s an example: What if the Core Data API changes? iOS 10 introduces `NSPersistentContainer`, which now encapsulates the Core Data stack for you. So instead of using an MOC, you might want to use a persistent container instead. So now, everywhere you need to use a persistent container instead of an MOC, you have to change your code. 

So, now you’re thinking, “Ok, smarty pants, how am I supposed to do anything about that? I can’t control what Apple does.” You can’t control what Apple does, but you can protect yourself from any third party code changes you are dependent on. You can accomplish this by putting the dependency behind your own layer of code that you control. Anytime you want to use this third party code, you do so through your own layer. If the third party code changes, you only have to change your layer and how it uses the third party code. 

Let’s look at a concrete example for the situation above. First, I would remove all the code in the app delegate that creates the Core Data stack into a new class. Let’s call it `DataStore`. For now, you can even add a shared property on it to make it easy to access (_this creates its own problems, but we’re just talking about the 2nd issue above for now_):

```swift
class DataStore {
   static let sharedInstance: DataStore = DataStore()
} 
```

You’ll notice that there’s no mention of Core Data or an MOC. Any code that uses `DataStore` has no direct dependence on Core Data and has no knowledge of it. `DataStore` doesn’t let you do anything yet, so let’s add some functionality. Let’s say your app has users and it needs to save them. Here’s a new version of `DataStore`

```swift
class DataStore {
   static let sharedInstance: DataStore = DataStore()
 
   init() {
       // Code to create Core Data stack
   }
 
   func addUser(username: String, password: String) -> throws {
       // Code to use Core Data to add a new user to the database.
   }
} 
```

Now, when somewhere in your app you need to create a new user: `try DataStore.sharedInstance.addUser("fred@anywhere.com", "12345")` 

That somewhere in your app doesn’t need to know anything about Core Data. If Core Data changes, it doesn’t care. If you decide to switch to Realm, it doesn’t care. If you decide all you really need is a text file, it doesn’t care. Other than the `DataStore` class, you have eliminated your dependence on Core Data.