---
title: What is Aki?
tags: dev, moo, lambdamoo, aki
layout: post
author: Vanity
---
This post tries to lay down a high level but precise view of what Aki is (and/or should be). 

### Aki is a LambdaMOO clone
I'm borrowing heavily from Pavil Curtis' ideas and also implementation. There is no doubt that this is a clone of LambdaMOO. Compatibility is not a goal or concern so in that regard it is not a port. And even though most current design choices align with LambdaMOO that's in no way a guaraantee for the future. Aki is still a bit in flux.

### Aki is a Go (golang) Excercise
I like Go. It's an amazingly tight language and I know I'm not the only one why learned it in a remarkably short time. Even after a few hours I was up and running writing useful stuff. It bascially has all the stuff I need/want:

    1. Great tooling and conventions
    2. Speed
    3. Awesome test/benchmarking/example support
    4. Easily portable from other languages
    5. Due to awesome literals (maps and slices are sweet)
    6. First class functions
    7. Green threads and channel based synchronization

It has some quirks but if you look at that list it has some killer features. You might not agree with all choices but in the end after a `gofmt` your code should look great (at least it does in Sublime).

### Aki Is Useful
There is no point in writing something for nothing. Eventually you should be able to spinup an aki instance in the cloud and have an easy access to run your own text-based game. Aki is not only useful for that though. The VM is quite powerful and should be easy to use indepently from Aki too. We can already demonstrate this in tests.

### What Is a MOO?
A MOO is kind of the predecessor of modern MMO's but more fancy. It is a database of objects that have properties and verbs attached to them. It also is a telnet-like server that reads text commands, executes them on the server and sends the results back (as text) to the client. 

So on the backend you have an object database and on the front end you have a command parser that tries to match commands that the player typed to verbs (and arguments) on objects that are accessible to the player at the time the time the command is executed. The client will utilize the backend database to move objects around and attach verbs and properties.

Before we go on to describe how Aki works it is important to realize that a MOO is basically a __scheduled__ VM as it has a notion of tasks. You can, for example, `suspend` a running programming and have it resume after a few seconds (which can be specified). Also, you can `fork` a program and have a whole piece of it run after a certain amount of time (in seemingly parallel) to your current program.

The `suspend` and `fork` are integral to a good MOO implementation. They are the most dangerous moving part because you cutting up the execution of a sequence of byte codes into multiple sequences of time. All the while managing a database of objects that may or may not be manipulated by any individual `fork` that is going on.

## First Steps
### Take 0
I thought about a channel of operations to the database. When every Go routine that would need to access the database go along a single channel this would mean no worries about locking. A channel of operations was not a good idea, getting stuff back to individual routines is not fun. Too much channel management.

### Take 1
Let's go old school instead, have little locks on each object. When any of the `db` functions would access an object they would lock them and then manipulate them. While it was great fun to have `defer` all over the place this didn't work at all. It was all cool if db functions would lock but what about programs? An object that was valid 3 instructions ago would suddenly be invalid despite the careful locks. This is not going to work.

### Final Take
Why not queue whole VM's? Before I was thinking small but then I realized I needed to start thinking bigger. If you can queue a `Program`, why not queue a `VM`. Then, when I realized how easy it would be to implement `fork` and `suspend` with a simple go routine things started opening up - this might work.

## Engine
At the heart of Aki lies a single channel with an inconspicuous name: `rt`:

    var rt = make(chan *VM)

This channel make sure that, even though there will be a multitude of go routines reading input and executing `Program` instances, the world will spin backwards. 

So what is a `VM`? Not much:

    type VM struct {
        Stack []*Activation
    }

Currently, it's nothing more than a stack of activation frames. Each one represents a verb call. Verbs can call other verbs so you can have quite a deep stack sometimes.

Now, to execute something, we need to setup a `*VM` with at least one `*Activation` on the activation `Stack` and we have something that can be eecuted. We just need to send it on the channel and that's it. 

There is only a single `rt` channel. All the routines will send their `*VM`'s and the channel will just process them in turn. Stuff that happens inside the VM is synchronous so there is no need to lock anything in the database. Order might not always be as expected for clients but you can't guarantee that anyway and there will be no ambiguous situations.