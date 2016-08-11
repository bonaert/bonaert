---
layout: post
title:  "An introduction to neural networks"
date:   2016-08-11 19:00:00 +0100
categories: jekyll update
---

In this series of posts, you'll learn what neural networks are, why they work and how you can use them in your projects. We'll use them to simulate simple functions like XOR to get a good feeling of why they work, and them we'll create an optical character recognition system (OCR).

### What are neural networks used for?

Neural networks (also known as *neural nets*) are a technique to simulate very complicated functions, that you can't express through equations. For example, it's very difficult to create a function by hand that takes in a image, applies some human-made rules, and then outputs the number it represents.

The advantage of neural nets is that they can **learn**. If need some training examples, and the network adjust itself to get more accurate results over time. 

Here's the fundamental structure of a neural network:

<img src="/assets/neuralnet.jpeg" class="center" title="Neural net structure">

There are 3 main parts:

1. The **input layer**, which are the inputs to the function you try to emulate (in an OCR system, the input would be the pixels)
2. The **hidden layers**, which does the necessary work to get the correct output *(note: there may be more than 1 hidden layer)*
3. The **output layer**, which is the output of the neural network with the given input

All the magic happens in the hidden layers.

### The simplest example

Let's say we want to emulate the XOR function. The rule behind the XOR function is quite simple: the output is 0 if the inputs are equal, 1 if they are different.

<img src="/assets/xor.png" class="center" width="320" height="200" title="XOR function">





q
q

