---
title: "Mixing Swift and Clojure in Your iOS App - Scittle"
date: 2025-12-24T18:10:57-07:00
draft: false
tags: ['Swift', 'Lisp', 'Apple Platforms']
---
In my [previous article](https://rodschmidt.com/posts/s7-scheme/), I showed how to embed a S7 Scheme interpreter in an iOS app. This time, I will show you how to embed a [ClojureScript](https://clojurescript.org) interpreter, or at least a dialect of it.

[Clojure](https://clojure.org) itself, runs on the JVM, and there’s not really a way to embed the JVM in an iOS app. Maybe once [swift-java](https://github.com/swiftlang/swift-java) gets rolling.

There is [GraalVM](https://www.graalvm.org/), which lets you compile java code to native code, but it doesn’t support compiling for iOS. [Babashka](https://babashka.org), which is a native Clojure dialect interpreter uses GraalVM.

There’s also [Jank](https://jank-lang.org), a native Clojure dialect, still in its infancy. I might try that next. That wouldn’t be an embedded interpreter. Instead you would use it to create static libraries which you could link into your app.

Babashka was created by the ever prolific [@borkdude](https://github.com/borkdude), who also created [SCI](https://github.com/babashka/SCI), the Small Clojure/Script Interpreter, upon which Babashka is built. SCI can be compiled to JavaScript, so theoretically, you could load it into JavaScriptCore and use it that way. I was unable to get that to work, even with Claude’s help.

## Scittle
Another project based off of SCI, is [Scittle](https://github.com/babashka/scittle), which is designed to let you execute Clojure/Script from browser script tags. Claude suggested I use this, and I was able to get it to work. The hardest part was downloading a precompiled to javascript version. Claude’s initial instructions were wrong, but I was able to download it eventually. My example repo at the end contains a pre-compiled version.

Once you have it downloaded, you just add scittle.js to your app as a resource.

## ScittleTest
I’ve written a test project called ScittleTest that shows how all this works. A key class in my example is `ScittleBridge`, which handles loading Scittle into JavaScriptCore and providing ways to hand it some ClojureScript and get it evaluated.

Another key class is `ClojureAPI`, which is a wrapper around `ScittleBridge`, It provides a high-level Swift API to your ClojureScript code. In your own app you would tailor this for your specific use-case.

ScittleTest shows how to:
* Provide a REPL
* Call Swift code from your Clojure code.
* Getting different kind of results from the Clojure code
* Loading Clojure code.

## Conclusion
That’s it. A working ClojureScript interpreter embedded in your iOS app. Another way to experience Lisp in your app.

You can find the ScittleTest repo [here](https://github.com/infiniteNIL/ScittleTest).
