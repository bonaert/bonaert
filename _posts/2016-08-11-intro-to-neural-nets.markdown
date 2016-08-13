---
layout: post
title:  "An introduction to neural networks"
date:   2016-08-11 19:00:00 +0100
categories: jekyll update
---

In this series of posts, you'll learn what neural networks are, why they work and how you can use them in your projects. We'll use them to simulate simple functions like XOR to get a good feeling of why they work, and them we'll create an optical character recognition system (OCR).

### What are neural networks used for?

Neural networks (also known as *neural nets*) are a technique to simulate very complicated functions: the kind you can't express through simple equations or rules. For example, it's very difficult to create a function by hand that takes in a image, applies some human-made rules, and then outputs the number it represents.

The beauty of neural nets is that you don't have to teach them the rules, they can **learn** by themselves. You feed them a lot of training examples, and the network adjusts itself to get more accurate results over time.

Here's the fundamental structure of a neural network:

{% include image.html
            img="/assets/neuralnet.jpeg"
            title="The structure of a neural net"
            caption="The structure of a neural net"
           %}

There are 3 main parts:

1. The **input layer**, which receives the inputs of the function you're trying to figure out (in an OCR system, the input would be the pixels of the picture)
2. The **hidden layers**, where the magic happens *(note: there may be more than 1 hidden layer)*
3. The **output layer**, whose values are the the output of the neural network

### Example 1: the XOR function

Let's first try to learn the XOR function. The rule behind the XOR function is quite simple: the output is 0 if the inputs are equal, 1 if they are different.

{% include image.html
            img="/assets/xor.png"
            title="XOR function"
            caption="XOR function"
            width="320"
            height="200"
           %}

## The simples rules of a neural network

A neural net computes the values of each layer successively, until we reach the last layer. But how do we compute the value of a node N in a layer?

### 1. Weights

The value of a node depends on every node of the previous layer. However, some nodes might be more important than others. Some might have a positive effect, while other have a negative one. So how do we capture this idea in maths?

{% include image.html
            img="/assets/weights.png"
            title="A neural net example"
            caption="Neural net with some weights (taken from <a href='http://stevenmiller888.github.io/mind-how-to-build-a-neural-network/'>this amazing post</a> by Steven Miller)"
            height="400"
           %}


The idea is simple: we associate to each connection between node N and the nodes in the previous layer a **weight**. Let's look at the first node of the hidden layer. We'll call that node N.

For each node in the previous layer, we multiply its value by the weight/connection with node N. We get 1 x 0.8 = 0.8 and 1 x 0.2 = 0.2. Then, we sum those numbers: we get 0.8 + 0.2 = 1 . This is the *net input* of the node N.

{% include image.html
            img="/assets/weightswithvalues.png"
            title="A neural net example"
            caption="The net inputs of the hidden layer (taken again from <a href='http://stevenmiller888.github.io/mind-how-to-build-a-neural-network/'>this amazing post</a> by Steven Miller)"
            height="400"
           %}

If we repeat this process through every layer, this would basically be a absurdly complicated way to do [linear combinations](https://en.wikipedia.org/wiki/Linear_combination). Unfortunately, linear combinations (which are linear functions) aren't enough to do OCR (or any non-linear function). We need something more powerful.

### 2. Activation functions

To simulate more complicated functions, we need to add non-linearity. We have to introduce something new: **activation functions**.

A very common one is the [sigmoid function](https://en.wikipedia.org/wiki/Sigmoid_function).

{% include image.html
            img="/assets/sigmoid.gif"
            title="The sigmoid function"
            caption="The sigmoid function"
           %}

It has some really nice properties:

 * The output is between 0 and 1 (automatic normalization)
 * Low values have images very close to 0, and high have values very close to 1 (it acts like a continuous binary switch)
 * It's differentiable everywhere (you'll see later why we need this)

After getting the *net input* of a node, we apply our activation function, giving the *value* of the node. Let's apply this to the example.

{% include image.html
            img="/assets/weightsvaluesactivation.png"
            title="A neural net example"
            caption="The values of the hidden layer (credit: <a href='http://stevenmiller888.github.io/mind-how-to-build-a-neural-network/'>this amazing post</a> by Steven Miller)"
            height="400"
           %}






q
q

