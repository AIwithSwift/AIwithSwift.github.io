---
layout: post
title: Building AI on iOS with Open Source tools
author: Jon Manning
tags: IRL
---

<img src="https://raw.githubusercontent.com/AIwithSwift/AIwithSwift.github.io/master/assets/images/oscon1.jpg"  class="postimage" />

This year at OSCON (LINK), we were thrilled to have the opportunity to present our workshop, _Machine overlord and you: Building AI on iOS with open source tools_. In this workshop, we explored several extremely cool libraries and practical techniques that make use of Core ML and Vision, which are the built-in machine learning and computer vision frameworks that ship with iOS 11.

In this post, we'll point you at several of the resources we used and demonstrated in the workshop. There's a lot of fantastic material out there for getting started with these topics!

## Turi Create

One of the biggest hurdles people encounter when they start experimenting with machine learning is the amount of time, effort and knowledge required to generate the models themselves. There are a lot of (excellent) tutorials out there on [TensorFlow](https://www.tensorflow.org/tutorials/), [Keras](https://machinelearningmastery.com/tutorial-first-neural-network-python-keras/), [Torch](https://pytorch.org/tutorials/beginner/deep_learning_60min_blitz.html), and so on. The problem is that these start with the fundamentals, and it takes time to build up to the point where you get to some of the more interesting applications.

This is where [Turi Create](https://github.com/apple/turicreate) comes in. Turi Create is a Python package that automates the creation of a very specific set of models, designed for certain tasks like image classification, object detection, style transfer, and text classification. Turi Create simplifies data handling, and automates the use of your computer's GPU, if you're using a Mac. It's designed to run on macOS and Linux, which makes it perfect for running on your development device. In addition, Turi Create is able to directly export models to the Core ML `.mlmodel` format, which means you can drop them directly into your project once they're trained. We can't recommend Turi Create enough.

## Core ML Store

Of course, training a model from your own data can take a long time, and sometimes what you need to get started is a model that already does what you're looking for. [coreml.store](http://coreml.store) is an excellent resource that contains a library of different models, all pre-trained and ready for use in Core ML.

It's worth pointing out that all of these models have their own license terms that you must follow. Many are under open source licenses, but some restrict their use to academic and personal use. Always read the terms!

We're extremely pleased to have had the opportunity to present this workshop at OSCON's 20th anniversary event, and we're looking forward to sharing more in the future!

<!--
## Use
This content was written by the authors named above, release for use under the [MIT License](https://opensource.org/licenses/MIT).
-->