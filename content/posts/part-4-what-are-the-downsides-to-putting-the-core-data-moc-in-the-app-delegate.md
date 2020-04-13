---
title: 'Part 4: What Are the Downsides to Putting the Core Data MOC in the App Delegate'
date: Tue, 01 Nov 2016 21:49:48 +0000
draft: false
tags: ['Apple Platforms', 'Software Development', 'Swift', 'Testing']
---

In [part 3](https://rodschmidt.com/part-3-what-are-the-downsides-to-putting-the-core-data-moc-in-the-app-delegate/), I talked about why putting the MOC in the app delegate makes any code that uses the MOC will be dependent on the app delegate and why that’s not a good thing. In [part 2](https://rodschmidt.com/what-are-the-downsides-to-putting-the-core-data-moc-in-the-app-delegate-part-2/#more-243), I talked about why putting the MOC in the app delegate is a violation of the Single Responsibility Principle. In [part 1](https://rodschmidt.com/what-are-the-downsides-to-putting-the-core-data-moc-in-the-app-delegate/), I talked about why putting the MOC in your app delegate makes you dependent on Core Data for your application’s persistence. Today, I Iike to address the 4th and final reason I gave in part 1. Here the 4th reason again:

> Any tests you write will be dependent on the App Delegate and Core Data and will be hard to test and slow as a result.

We’ve already shown in part 3, how any code you write will be dependent on the App Delegate and Core Data, so any tests you write will also be dependent on them. Any test you write, in order to use the MOC in the app delegate will have to instantiate the app delegate just to run. With the way XCTest runs tests that means before every test runs the app delegate will have to be instantiated and the Core Data stack created and initialized. Any test you write using Core Data will typically be reading and writing data to or from disk. You typically want to write some data to disk and then read it, possibly write it again, and then delete it to cleanup after a test. That’s a lot of reading and writing to to disk and we all know that is slower than accessing RAM. Unit tests need to be fast so we can run them constantly after making a change, especially if you’re doing TDD and you’re doing the TDD waltz of writing a failing test, making the test pass, and then refactoring. After each step you want to run your tests so they need to be fast. With a lot of tests, going to disk all the time is going to add up and slow things down significantly. So how do we fix this? First we do what I suggested in [part 1](https://rodschmidt.com/what-are-the-downsides-to-putting-the-core-data-moc-in-the-app-delegate/), which is create a DataStore class that contains all the Core Data code for you app. Here’s that class again:```
class DataStore {
   static let sharedInstance: DataStore = DataStore()
 
   init() {
       // Code to create Core Data stack
   }
 
   func addUser(username: String, password: String) -> throws {
       // Code to use Core Data to add a new user to the database.
   }
} 
```What’s next? Well, Core Data does support memory only contexts, so, that’s a possibility. Also, with some test, we don’t really care that data gets written or read from the database. We just want to know that the right method was called to do the reading or writing, so we could change DataStore to just record that a method was called. What we need is a way to put in different `DataStore` implementations for different tests. The way to do that with Swift is with protocols. First let’s rename our `DataStore` class to `CoreDataStore`. Next, let’s create a protocol called `DataStore`. It will look like this:```
protocol DataStore {
    func addUser(username: String, password: String) -> throws   
} 
```Let’s also change `CoreDataStore` to implement `DataStore` and remove the `sharedInstance` property:```
class CoreDataStore: NSObject, DataStore {
   init() {
       // Code to create Core Data stack
   }
 
   func addUser(username: String, password: String) -> throws {
       // Code to use Core Data to add a new user to the database.
   }
} 
```We removed the `sharedInstance` property because we need to be able to control what class is used to instantiate a `DataStore` implementation. We can’t do that very easily or in a flexible manner with a singleton. Now we can create an `InMemoryDataStore` class and a `SpyDataStore` class:```
class InMemoryDataStore: NSObject, DataStore {
   init() {
       // Core to create in memory core data stack
   }
   
   func addUser(username: String, password: String) -> throws {
        // Code to use in memory context to add new user to database
   }
}

class SpyDataStore: NSObject, DataStore {
    var addUserCalled = false

    func addUser(username: String, password: String) -> throws {
        addUserCalled = true
    }
} 
```We need to change the app delegate to instantiate an instance of `CoreDataStore`, but you will notice the type is `DataStore`. `DataStore` will be used all over the rest of the app. The only place you will see `CoreDataStore` is when it is created. So somewhere in the app delegate you would have code like this:```
let dataStore: DataStore = CoreDataStore() 
```and then you would need to pass `dataStore` to anybody that needed it such as your root view controller who would forward it other view controllers, etc. Finally, to speed up those tests, we either create an instance of `InMemoryDataStore`, or `SpyDataStore`, depending on our needs, like so:```
func testAddUserWasCalled() {
    let dataStore = SpyDataStore()
   
    // pass dataStore to all the objects that need it that our involved 
    // in the test.

    // Some operation on a view controller or other object that should 
    // cause addUser to be called

    XCTAssert(dataStore.addUserCalled, “addUser not called on data store”)
} 
```Now our tests will run fast and we can change the implementation of how we store data with ease. This finishes the series on why you shouldn’t put the Core Data managed object context in the app delegate. I hope I was able to help you see why this is not a good idea and present to you a better way. If you have any questions or opinions, please feel free to comment below or email me.