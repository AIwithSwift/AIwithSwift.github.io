---
layout: post
title: Machine Learning ...without the machine (Drafting)
author: Mars Geldard
tags: IRL, Algorithms
---

<img src="https://raw.githubusercontent.com/AIwithSwift/AIwithSwift.github.io/master/assets/images/mars-speaking.jpg" class="postimage" />

The authors of this site were lucky enough to spend the past week at one of our favourite conferences: [/dev/world](https://devworld.com.au). Focused on development for Apple technologies, this conference is now in its eleventh year and has a growing community that come from all over Australia and New Zealand to attend in Melbourne each year.

I was lucky enough to speak at /dev/world this year for the first time, with a talk titled [*Machine Learning ...without the machine*](https://devworld.com.au/sessions/15.html). My description argued that it is "crucial to understand the steps being done by a computer when you use tools like CoreML and CreateML in order to make informed decisions in the human-driven design and data selection stages that come before model training" and that I could teach underlying principals with only visual examples and very little actual *maths*.

Despite using the evil M-word--and being up against the very talented [Keith Lang](https://devworld.com.au/sessions/20.html) in the other talk stream--I had high attendance and received very positive feedback. I enjoyed it so much I thought I would summarise and expand upon the talk content here, including some resources that can be used to learn more.


## Terminology

To begin we discussed the possible issues some may have with the talk title, to get any pedantry out of the way. The title of this talk uses **Machine Learning**, but the distinction between ML and **Artificial Intelligence** is often defined as AI is the initial presentation of some knowledge, ML is the integration of feedback to learn from its mistakes. Quite a few older systems had AI without ML at all and, depending on the purpose, sometimes that is all you need. The field that overarches both of these concepts--and more, including general presentation and manipulating of data--is **Data Science**. So this looks something like:

<img src="https://raw.githubusercontent.com/AIwithSwift/AIwithSwift.github.io/master/assets/images/terminology.png" class="center" />

Cool? Cool. So this talk's content *is* artificial intelligence, and it is *a type* of data science, but it is only the *first step* in machine learning. In fact, only some of the methods discussed can "learn" after deployment.

A few common terms appear in descriptions throughout:
* **Observations** refer to a set of past data collected that future knowledge will be derived or assumed from. **An observation** is a single entry in this past data, say a line in a table.
* **Input** refers to the single entry that is yet to be processed--sorted, placed or classified. This is the thing you are trying to get new knowledge of or for.
* **Attributes** are the things known about each entry, both observation and input.
* **Outcome** or **class** are used interchangeably to refer to the thing *you want to know* about each entry, present in the observations but still required for the input. This is often one of a set of pre-chosen options.

So, given a set of **observations**, we can process **input** with the same **attributes** to get its assumed **class**.
Given a set of **cat and dog pictures**, we processed **a new image**, extracted its **features and colour composition** and found the computer thought it was a **dog**.

Next, let's discuss three simple terms used to discuss some *purposes* for machine learning AKA the types of questions you can answer using different methods. The first is **Descriptive**, which is asking "what does my data say has happened in the past?". This is the aim of methods such as clustering that *describe and summarise* your data, often with the explicit purpose of *identifying significant or anomalous observations*.

The second is **Predictive**, which is asking "based on what has happened in the past, what will happen now?". This is the aim of methods such as classification or regression, who seek to apply more information to some input based on the information associated with past observations.

The third is **Prescriptive**, which is asking "based on what has happened in the past, what should I do now?". This often builds upon predictive systems, with the addition of contextual knowledge that dictates appropriate response to predicted outcome.

Moving onto different kinds of values we would encounter when discussing the strengths and applications of different algorithms, we began with **numerical** versus **categorical** values. This is a pretty intuitive distinction: numerical is number, categorical is category. If you are crunching an online store with a database full of movies then the price of each movie would be numerical, while the genre of each would be categorical. A third type of value common in observations is unique identifiers, but these are not discussed here as they should be omitted during analysis.

These numerical and categorical value types can then be broken down further: each has two sub-types that should be regarded differently in many cases. Numerical values can be **discrete** or **continuous**. Put simply, the difference is that if there is a set number of values something can be, it is discrete. If not, it is continuous. For example, a set of all whole numbers between 0 and 100 would be discrete, whereas a set of every decimal between 0 and 1 would be continuous as it is theoretically infinite.

This becomes a bit more complicated when we consider mathematics defines sets such as natural numbers--which contains all positive integers--are defined as *discrete*. This is because even though they theoretically go on forever, you can take two numbers from within it and count all numbers that go between them. In the case of real numbers--effectively any number of any length and decimal that occurs at any point along the number line--you can fit an infinite amount of other real numbers between any two picked. For the purposes of machine learning, different methods will work better depending on if after defining an upper and lower boun, all the values between them can be counted or not.

Categorical values can be either **ordinal** or **nominal**, where ordinal refers to values which have some order or relations between them that would imply connections between some are closer than others. For example, if you're doing some analysis or prediction based on data from several locations to predict something in one place, places near to it should be considered heavily while places far away should only be considered if very similar in demographic. This creates a concept of weighting or order among these values, even though they are not numbers, that would be beneficial for your analysis to be aware of. Though locations often cannot be simply swapped out for numbers because it would be difficult to map relations that occur in 3-dimensional space, in many cases of analysing ordinal values the underlying system will be treating them as representative numbers for ease of calculation. Nominal is the opposite, each value is equally different to all others. For example, in the set of primary colours each is equally and immeasurably different from the other. Another popular example of nominal values is binary categories, such as true/false. These cannot be used with methods that rely on a measure of similarity or proximity, as most do.

Some values are not so easy to intuit. For example, postcodes should be treated as Categorical>Nominal values, as while they are represented as *numbers* and have *some vague order* you would not want to train a system to think that the distance between say Adelaide (5000) is closer to Brisbane (4000) than it is to Hobart (7000), which is almost 600km closer. Depending on what you are using the input data for, it may be more useful to replace postcode entries with new state and suburb columns, or abstract ranges for each state that correspond to inner city, suburban and rural areas. Since some states are adjacent and travelling from rural areas to a city would often take you through the suburbs, these would now have become theoretically ordinal values.

Knowing how to classify your input values in this way--and recognising where another type of value would serve you better--is a key part of method and data selection that will allow the creation of more robust and useful machine learning models.

## Classification

Classification is the application most commonly associated with machine learning in the eyes of the broader public, particularly in the case of one of the most widely-used methods: neural networks. Many of those around us who are aware of technological trends but not particularly technical themselves even believe that neural networks are how all machine learning works, that this is the only method you can use. It's not. This one just has a catch name that people can use cool brain pictures with and claim that "no-one knows how it works" because it's the hardest to expose the internal processes of.

There are many others, many of which are simple enough to demonstrate on paper. They are used to drive many systems, such as recognisers made to interpret voice input, or label images or video content; recommendation systems present in online stores, social media, streaming platforms and more; as well as a number of decision-making components that make large and complex systems work smarter. So many clever and varied applications, the list could go on forever--basically think of everything that could benefit from responding dynamically based on experience.

Let's look at a handful and compare.


### Methods

#### Naive Bayes

Naive Bayes is the method of taking some input *I* and some set of classes {*C1*, *C2*, ... , *Cn*} and guessing the class that *I* should be based on a set of previously-classified instances *E*. This is done by using **Bayes' Theorem** to calculate the relative probability *P* of it being each class from the set of options.

<img src="https://raw.githubusercontent.com/AIwithSwift/AIwithSwift.github.io/master/assets/images/bayes.png" class="center-mid" />

For example, say a person has a table of different types of food they have eaten lately. A friend has proposed they go out to Thai food again tonight, but they might disagree if they think they're not going to like it. So they employ the Naive Bayes method to classify the **likely outcome** of their Thai food experience from options {👍, 👎}.

<img src="https://raw.githubusercontent.com/AIwithSwift/AIwithSwift.github.io/master/assets/images/thai.png" class="center-small" />

First they used Bayes' Theorem to work out the relative probability of Thai being 👍:<br>

<img src="https://raw.githubusercontent.com/AIwithSwift/AIwithSwift.github.io/master/assets/images/bayes-thumb1.png" class="center-mid" />

<img src="https://raw.githubusercontent.com/AIwithSwift/AIwithSwift.github.io/master/assets/images/bayes-answer1.png" class="center-mid" /><br>

Then they work out the relative probability of Thai being 👎:<br>

<img src="https://raw.githubusercontent.com/AIwithSwift/AIwithSwift.github.io/master/assets/images/bayes-thumb2.png" class="center-mid" />

<img src="https://raw.githubusercontent.com/AIwithSwift/AIwithSwift.github.io/master/assets/images/bayes-answer2.png" class="center-mid" /><br>

Now it's simple. The most *probable* is whichever one has a greater relative probability. In this case it is 👍, so we can fill that in and say they're probably happy with their friend's choice of food.

<img src="https://raw.githubusercontent.com/AIwithSwift/AIwithSwift.github.io/master/assets/images/thai2.png" class="center-small" />

But if we look at the input again, we probably could have just asked which outcome most commonly corresponded with the given input (Thai) in the past data. We would have gotten the same answer from many people just from visual inspection. It is a very straightforward assumption: the outcome that has most often corresponded with this input in the past will be the outcome in this case. The only reason we call this "artificial intelligence" is because it's a method often employed with complex parametric input that would be unfeasible for a human to do on paper over and over again for each input. So this AI is really just automated maths, as you'll soon see the even the most mysterious and complex machine intelligence always is.

With most classification approaches being this type of **probabilistic classification**, it's handy to make educated guesses but understandably falls apart in cases where outside context changes or there are too many input or output options to get a significant measure. They cannot distinguish between valid observations and anomalies in past data, and will perpetuate each with equal consideration. This is unlike an approach such as a neural network that will amplify differences in input distribution, minimising this effect. Probabilistic classification methods also suffer significantly with continuous values in their input or output, such as in cases where you are trying to classify things with numbers out of a large set of possibilities. In these cases, a related approach to classification is needed: regression.


#### Decision Trees

**FINISH**


Random Forest is a name given to a method used to reduce inaccuracies in decision trees introduced by random selection of branching points where there were options of equal validity. Instead of making just one tree, they will use the same past data to make many, each using different seeds or weights to mix up the random selection made at any points of equality. Then they will classify the same input with each tree and take the most common outcome--or rarely the average of the outcomes, depending on its purpose.


#### Distance Metrics

Here, I must take a moment to discuss a concept required for some of the classification methods to follow: distance metrics. Because so many methods of machine learning rely on taking a set of input attributes--basically one row of a table, missing its class or outcome entry--and transforming it into a point that can be compared in some n-dimensional space, we require a way to measure the *distance* between points we may not be able to visually represent or conceptualise.

Now these metrics are a point of much contention between specialists about which is most effective or correct under different circumstances, so we will just discuss some very common ones. Know that there are many more, both derivative of these and entirely different.

First, we have a method used to compare inputs with *nominal* inputs. Because their possible values have equal similarity and dissimilarity, comparisons are made on whole sets of values at once to produce a rating of their similarity.

The most common one *I have used*, and likely the most straightforward is **Jaccard** distance. This takes two inputs and calculates the "distance" between them as a score of overlap divided by difference.

<img src="https://raw.githubusercontent.com/AIwithSwift/AIwithSwift.github.io/master/assets/images/jaccard1.png" class="center-mid" />

So if we get two data entries, say two people listings with columns for gender, education level and country of origin.

<img src="https://raw.githubusercontent.com/AIwithSwift/AIwithSwift.github.io/master/assets/images/jaccard2.png" class="center-mid" />

Then we sort out the *set representation* (basically don't list duplicates) of what they have in common and what they have between them overall.

<img src="https://raw.githubusercontent.com/AIwithSwift/AIwithSwift.github.io/master/assets/images/jaccard3.png" class="center-mid" />

This is then a simple division of similarity, subtracted from one to get the inverse or dissimilarity.

<img src="https://raw.githubusercontent.com/AIwithSwift/AIwithSwift.github.io/master/assets/images/jaccard4.png" class="center-mid" />

Second, we have methods used to compare inputs that are either *numerical* or *ordinal* in some way.

One possible metric for this is **Euclidean** distance.

<img src="https://raw.githubusercontent.com/AIwithSwift/AIwithSwift.github.io/master/assets/images/euclid-algo.png" class="center" />

The Euclidean distance between two sets of values is equal to the square root of the squared distance between first value in each set, plus the squared distance between the second value in each set, plus the squared distance between the third value in each set, and so on. The distance between two single values is usually just the numerical difference,  or degrees of separation for ordinal values that can have their proximities mapped or be assigned corresponding indices.

Represented in only two dimensions for simplicity, this forms something like this:

<img src="https://raw.githubusercontent.com/AIwithSwift/AIwithSwift.github.io/master/assets/images/euclidean.png" class="center-small" />

Another possibility is **Manhattan** distance.

<img src="https://raw.githubusercontent.com/AIwithSwift/AIwithSwift.github.io/master/assets/images/manhat-algo.png" class="center" />

The Manhattan distance between two sets of values is equal to the distance between first value in each set, plus the distance between the second value in each set, plus the distance between the third value in each set, and so on. The distance between two single values is still usually just the numerical difference or degrees of separation.

This forms something like this:

<img src="https://raw.githubusercontent.com/AIwithSwift/AIwithSwift.github.io/master/assets/images/manhattan.png" class="center-small" />

So say we use a well-used example in data science: flower measurements. So we have some flowers. For some hypothetical further ML purpose, we first want to know which pair are the most similar or *closest* to each other among them.

<img src="https://raw.githubusercontent.com/AIwithSwift/AIwithSwift.github.io/master/assets/images/flowers.png" class="center" />

So we can *normalise* the values, so they have an equal range to compare. This is done by changing the upper and lower bounds of possible values to 1 and 0 respectively, leaving all values in between them.

<img src="https://raw.githubusercontent.com/AIwithSwift/AIwithSwift.github.io/master/assets/images/flowers2.png" class="center" />

Now we can use our two distance metrics we have learned for numerical values from above. First, we get pairs of values for each pair of flowers.

<img src="https://raw.githubusercontent.com/AIwithSwift/AIwithSwift.github.io/master/assets/images/pairs.png" class="center-mid" />

Then we can try with our Euclidean distance, using the square root of the squared difference between each pair of values.

<img src="https://raw.githubusercontent.com/AIwithSwift/AIwithSwift.github.io/master/assets/images/distance1.png" class="center" />

And we can try with our Manhattan distance, using the absolute value (ignore any negatives) of the difference between each pair of values.

<img src="https://raw.githubusercontent.com/AIwithSwift/AIwithSwift.github.io/master/assets/images/distance2.png" class="center" />

At this point you might see that Manhattan distance is good for enhancing differences, while Euclidean distance smooths differences. These are suited and used for different purposes with different accompanying algorithms and machine learning methods.

<img src="https://raw.githubusercontent.com/AIwithSwift/AIwithSwift.github.io/master/assets/images/distance3.png" class="center-mid" />

Either way, our most similar flowers are 🌼 and 🌸. Easy? Easy.

Using the types of calculations covered in this section we can now calculate the distances between abstract sets of multiple, possibly even non-numeric values.


#### Nearest Neighbour

Nearest neighbour (or K-nearest neighbour) is a method that utilises a theory similar to the old saying "show me who your friends are and I'll tell you who you are". Its core assumption is that the class that applies to new input will the class that most applied to past instances with the most similar attributes. This is really just a re-framing of the probabilistic approach covered earlier.

It works like so: say we plot some observations visually--this required something with very few attributes. Given the below example, where we have plotted some worker's annual incomes and average hours worked per week, let's say the colours/shapes correspond to something like whether or not they have joined the worker's union. They just hired a new employee, marked by a question mark, and the workers want to know whether or not they're likely to join.

<img src="https://raw.githubusercontent.com/AIwithSwift/AIwithSwift.github.io/master/assets/images/K0.png" class="center-mid" />

So we can say his behaviour is likely to correspond to the current employee with the most similar circumstances. If 🔺 is not joined and 🔹 is joined, and their *nearest neighbour* is 🔺--using the some distance measure like those discussed earlier--then we can say they're likely to be the same.

<img src="https://raw.githubusercontent.com/AIwithSwift/AIwithSwift.github.io/master/assets/images/K1.png" class="center-mid" />

*However* this method of just using a single neighbour would be incredibly prone to outliers. So what do we do when faced with outliers in data science? **We average!**

In this method, we take more than one neighbour and use the most common class found. The number of neighbours we select *K* is effectively random* can result in vastly different classifications, as seen below.

<img src="https://raw.githubusercontent.com/AIwithSwift/AIwithSwift.github.io/master/assets/images/K2.png" />

\**This is termed "bootstrapping" in data science, likely due to experts not wanting to say that a method requires you to select something at random as they're equally likely to work or not.*

This selection can be made less error-prone in a few ways. Though there are arguments that the best *K* is an odd number around the square root of the total number of points in your past data, there are other methods that employ what is effectively trial-and-error. But, no matter what you do, it should be known: accuracy of both nearest neighbour and a method for selecting *K* will vary wildly when applied to different datasets, and with complex input it is often difficult to see when it has gone wrong.

Note that this method can also be used to classify between a set of more than two class options, but will very likely become much less accurate as the number of options grow. This is termed *the curse of dimensionality*.


#### Support Vector Machine

Similar to nearest neighbour, support vector machine is an approach that utilises theoretical placement of each observation in an *n*-dimensional plane, where *n* is the number of attributes. **FINISH**

#### Neural Network

**FINISH**


### Applications

**FINISH** Image recognition, sound recognition


## Clustering

### Methods

#### Hierarchical

**FINISH** Agglomerative versus divisive


#### K-means

#### DBSCAN

#### Mean shift

### Applications

Compression


## Regression

### Methods

**FINISH** Logistic Regression, Other


### Applications

## Extras

So this was a very quick introduction to some machine learning terminology and concepts. This topic is a favourite of mine and I could talk/write about it all day but there are more types and methods of machine learning that could fit in one--even very long--blog post. Nevertheless, it is a fascinating topic that alienates many who assume more complexity than there is or just don't know where to start; I am glad to hear that the talk this summarises sparked passion in some and encourage anyone interested to seek out the abundance of resources available on the internet to learn more. Here are a few of my favourites:

* [Kaggle](https://www.kaggle.com), a site where you can learn and hone data science skills by using provided data sets, exploring example projects or entering community-made contests.
* Google's [Machine Learning Crash Course](https://developers.google.com/machine-learning/crash-course/) has ~15 hours of content and exercises hand-selected from
* If you're willing to invest, there are also relatively cheap online university courses on data science from a number of reputable institutions, such as [Foundations of Data Science at Berkeley](https://www.edx.org/professional-certificate/berkeleyx-foundations-of-data-science#courses).
their extensive [AI education site](https://ai.google/education).
* There are also a number of more human experience-centric write-ups out there, such as an AI-focused MIT researcher's [*Lessons from My First Two Years of AI Research*](http://web.mit.edu/tslvr/www/lessons_two_years.html).
* Or just have a go! Download [Rstudio](https://www.rstudio.com), some [CoreML](https://developer.apple.com/documentation/coreml) bits, or even a dedicated application like [Weka Explorer](https://www.cs.waikato.ac.nz/~ml/weka/gui_explorer.html) and play around with info from blogs and documentation on Google until something clicks. Some people's brains just work that way.

<img src="https://raw.githubusercontent.com/AIwithSwift/AIwithSwift.github.io/master/assets/images/comic.jpg" class="center-mid" />

*A lovely audience member's charicature of me speaking, math bits flying around my head.*
