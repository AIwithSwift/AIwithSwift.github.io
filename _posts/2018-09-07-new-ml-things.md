---
layout: post
title: Apple's latest AI tools
author: Paris Buttfield-Addison
tags: Apple
---

At WWDC this year, Apple announced CoreML 2, the latest version of their machine learning framework. CoreML 2's release is, presumably, just around the corner! What does it bring to the table? Many things! I'm most excited about two of them: smaller models, and CreateML.

One of my favourite new features is smaller models! In CoreML 2 you can quantize your CoreML models; quantizing basically involves reducing the possible ranges of things, and boils down to models being made much, much smaller. Smaller models means less memory consumption—-something that's super important on a mobile device!--and faster calculations using your model.

CoreML 2 also ships with CreateML, a system, integrated with Playgrounds in Xcode (on macOS) that makes it much easier to make simple models, which in turn makes it much easier for people to get started with machine learning.

CreateML lets you drag and drop things you want to train—-say, images——into an Xcode window, define some categories, and train it. You can then easily export a model for use in a full CoreML app. It's kind of amazing.

We'll be writing up a few tutorials on the basics, in the coming months. In the mean time, check out <a href="https://developer.apple.com/machine-learning/">Apple's Machine Learning page</a>!

