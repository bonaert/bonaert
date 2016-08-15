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

Repeating this process for the last layer, we get the following result:

{% include image.html
            img="/assets/firstresult.png"
            title="The final result"
            caption="The prediction of our neural network (credit: <a href='http://stevenmiller888.github.io/mind-how-to-build-a-neural-network/'>this amazing post</a> by Steven Miller)"
            height="400"
           %}

## How do we improve the network?

To improve the network, we can either change the structure of the network or the weights. The structure is usually chosen at the start by humans, and doesn't change while the network learns. Instead, the **weights slowly change** with every training example until the prediction become accurate.

But how do we know how to change the weights? *This parts has a good chunk of maths, so skim you don't want to go into every detail.*

The key idea is that we can define a **cost function**, and see how changing a weight impacts that cost function. Yes, that means good old calculus and derivatives/gradients.

If increasing a certain weight increases the error, then we do the opposite: we decrease the weight. This is fundamental idea behind [backpropagation](https://en.wikipedia.org/wiki/Backpropagation#Motivation), the most commonly used technique to improve neural networks.

First, let's define our cost function:

<script src='https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML'></script>

$$ E = \frac{1}{2}(Actual - Predicted)^2 $$

We put the $$\frac{1}{2}$$ because when we'll derive it later on, it will cancel out. Okay, now we need to see how changing weights impacts that function.

Let's start with the weights of the last layer (currently $$0.3, 0.5$$ and $$0.9$$). Using the wonderful chain rule, something that looks quite hard to figure out is actually really simple.

Let's start with the top weight, we'll call it $$w_1$$.

$$\frac{\partial E}{\partial w_1} = \frac{\partial E}{\partial output} * \frac{\partial output}{\partial input_{net}} * \frac{\partial input_{net}}{\partial w_1}$$

Here,

* $$input_{net}$$: the net input of the node in the last layer
* $$output$$: the value of the node (the result of applying the activation function on the net input).

Let's then figure out the value of each of the three terms in our example.

<br>

### Component  1

$$ \frac{\partial E}{\partial output}
= \frac{\partial (\frac{1}{2} * (Actual - Output)^2)}{\partial output}
= - (A - output) = - (0 - 0.77) = 0.77$$

You can see that the $$\frac{1}{2}$$ term cancelled the $$2$$ from differentiation, making everything simpler. That's why we added it.

<br><br>

### Component 2
Then, we have to compute $$ \frac{\partial output}{\partial input_{net}} $$. Since the $$output = a(input_{net})$$ where $$a$$ is our activation function, then $$\frac{\partial output}{\partial input_{net}}$$ is actually really simple: it's the derivative of the activation function evaluated at $$input_{net}$$.

The cool thing about the sigmoid function is that its derivative is really simple:

 
$$ a'(x) = a(x) * (1 - a(x)) = output * (1 - output) $$

In our example,

$$ \frac{\partial output}{\partial input_{net}} = a'(input_{net}) = output * (1 - output) = 0.77 * (1 - 0.77) = 0.17 $$

<br><br>

### Component 3

Last, we need to figure out the value of $$\frac{\partial input_{net}}{\partial w_1}$$. Again, this is surprisingly easy, if we think about it for a minute.

What is $$input_{net}$$? It's the $$ \sum $$ weight * input for every node in the previous layer. For the output node, then

$$ input_{net} = w_1 * input_1 + w_2 * input_2 + w_3 * input_3$$

Therefore, the derivative for a certain weight is simply the associated input. In other words,

$$ \frac{\partial input_{net}}{\partial w_1} = input_1 = 0.73 $$

<br><br>

### Bringing it together...

we then have that

$$\frac{\partial E}{\partial w_1} = \frac{\partial E}{\partial output} * \frac{\partial output}{\partial input_{net}} * \frac{\partial input_{net}}{\partial w_1} = 0.77 * 0.17 * 0.73 = 0.095$$

We can then see that if we increase the weight, then the error rate will increase. Obviouly, we have to do the opposite: decrease the weight.

But by how much? Usually, we multiply that derivative with a **learning rate**, and then subtract that from our weight. In other words

$$ weight_{new} = weight_{old} - \alpha * \frac{\partial E}{\partial w_1}$$

where $$\alpha$$ is the learning rate. Let's $$\alpha = 0.2$$ as our learning rate and apply it on the weight. Then,

$$w_{1, new} = w_{1, old} - \alpha * \frac{\partial E}{\partial w_1} = 0.3 - 0.2 * 0.095 = 0.281$$

This might seem like a small change, but it actually isn't, since neural networks need to train over thousand of examples to get good results. If each example made the weights bounce all over the place, then you'd have a hard time converging them to the optimum. Choosing a good learning rate (and a good structure for the network) is called [hyperparameter tuning](https://en.wikipedia.org/wiki/Hyperparameter_optimization) and is a magic art by itself.

### One little problem...

We were able to figure out how to improve the weights of the last layer. What about the weights of previous layers? Let's look at our fundamental equation again:

$$\frac{\partial E}{\partial w_{hidden}} = \frac{\partial E}{\partial output_{hidden}} * \frac{\partial output_{hidden}}{\partial input_{net}} * \frac{\partial input_{net}}{\partial w_{hidden}}$$

Thankfully, the 2nd and 3rd term stay the same and are still easy to compute. Unfortunately, the first term becomes difficult to compute? Imagine we're trying to figure out

$$ \frac{\partial E}{\partial output_{hidden}} $$

for an output that's 20 layers away of the output. How could we figure it out?
The answer to that question also explain why the algorithm is called backpropagation... if you want to figure it out, check out my next post (*not published yet*).