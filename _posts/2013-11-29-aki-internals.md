---
title: Aki Internals
layout: post
author: Vanity
tags: dev, moo, lambdamoo, aki
---
I tried to recreate LambdaMOO in various languages. Sometimes multiple times. I think this is my third try in Go but I also did some half-assed implementations in C# and in lesser degree in Python. The original is written in C and during the time studing that code I more or less had to learn it. 

I don't even think I had to work with plain C before. I know I did some stuff in C++ but that was at the academy and the most fancy thing I wrote was a doubly linked list. The assignment was to write just _a linked list_ but I couldn't get an efficient implementation out of the specified API in the few minutes I wanted to spend on it so I just wrote a double linked linked list instead. My teacher didn't like it of course.

So my C is not sharp... It's quite blunt but I have been studing this code and finally I feel I can make some kind of sense of the meaning behind it all. There is just one small thing that is not to be forgotten: we are not working in C.

## Serving
I stabbed at this problem from multiple angles but the insight came when I started to thnk about how to serve all (all 4 or maybe 5) connections that I need to handle. Go is like a siren that lures you in. Why?

1. It's stupidly easy to setup a TCP server
2. It's stupidly easy to setup a TCP server

There are so much examples but it all boils down to the fact that you can `go` a routine for each connection and have them all do their thing. It will look a bit like this:



