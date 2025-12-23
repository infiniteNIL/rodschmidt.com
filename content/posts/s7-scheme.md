---
title: "Mixing Swift and Lisp in Your iOS App - S7 Scheme"
date: 2025-12-22T21:07:11-07:00
draft: false
tags: ['Swift', 'Lisp', 'Apple Platforms']
---

I am a fan of [Lisp](https://en.wikipedia.org/wiki/Lisp_%28programming_language%29). I was first attracted to it because it was the original language of artificial intelligence because of its ability to manipulate symbols and its own code. I spent  a whole year using Lisp in a series of courses on artificial intelligence in college. I became quite adept at using it and quite enjoyed using it as well.

## My Obsession
Lately, I’ve become kind of obsessed with being able to use Lisp in my iOS apps. I want to put a Lisp interpreter in my app and program my business logic in Lisp. Every once in an awhile I get the urge to figure it out, try, and fail. This keeps happening.

I know it’s pretty impractical, but the obsession is there. Besides, there are advantages to using higher level languages to better represent your problem space. Lisp excels at creating domain-specific languages (DSLs) for whatever problem you’re working on.

The current king of the Lisp world is Clojure, and that is the obvious place to start. However, it’s not that easy to embed a Clojure interpreter in your app. Believe me, I’ve tried.

I’ve tried embedding interpreters for [Clojure](https://clojure.org), [ClojureScript](https://clojurescript.org), and various dialects of [Scheme](https://www.scheme.org). Despite some people having accomplished it, I've had no success. Their examples were out of date, often over a decade old, and I couldn’t get anything to compile or integrate with the modern version of an Xcode project for an iOS app. A lot of the difficulty is around the fact that the library for a Lisp implementation needs to be compiled for iOS. A lot of the dialects support macOS and it is easy to compile for macOS. iOS is harder because you’re basically cross-compiling and there’s a lot of configuration involved.

Recently though, I’ve made some breakthroughs. With a little help from some LLMs, I was able to get multiple variations of a Lisp interpreter into my iOS app.

## S7 Scheme
The first one I want to talk about is [S7](https://ccrma.stanford.edu/software/snd/snd/s7.html) scheme. At first, I tried [Chibi](https://synthcode.com/scheme/chibi) Scheme, but even with Claude, I couldn’t get that to work. Claude suggested S7, so I switched to that. It’s probably the easiest to integrate because it consists of just 2 files: s7.h and s7.c. Just add those 2 files to your project, add a bridging header, compile, and you should be good to go. Except, it’s not quite that simple when compiling for iOS.

The first error you get is _“system() has been explicitly marked unavailable here.”_ The C `system`() call is not available on iOS because of the sandbox. To fix that just add  `#define WITH_SYSTEM_EXTRAS=0` to s7.c. Make sure to add it before other \#includes that may need it.

Next, you’ll get a bunch of warnings when compiling s7.c. These are just annoying and harmless. To suppress these, go to the Build Phases of your project, open up the Compile Sources phase and double-click on s7.c. A popover will appear where you can enter some compiler flags that will just apply to s7.c. Add these flags:

> -Wno-shorten-64-to-32 -Wno-conversion -Wno-shorten-64-to-32 -Wno-unreachable-code -Wno-conversion -Wno-comma -Wno-conditional-uninitialized

With that, everything will build cleanly.

## A Swift Wrapper
My example includes a Swift class called `S7SchemeInterpreter`. It handles all the bridging between calling S7’s C API and Swift. It handles the following:
* initializing and deinitializing S7
* evaluating Scheme code and getting the result back as a Swift type.
* loading Scheme code
* defining Scheme variables from Swift
* calling Scheme functions from Swift
* Converting S7 types to Swift types and vice-versa.
* Registering Swift functions that can be called from your Scheme code.

# Conclusion
That’s it. A working scheme interpreter embedded in your iOS app. Create DSLs. Provide an extension language to your users. Write code in a dynamic language and functional language.

A repo with a working example is [here](https://github.com/infiniteNIL/S7Test).
