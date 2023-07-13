---
title: "Is it sweet or savory?"
date: 2023-07-13
---

Earlier this past winter I took the [fastai course](fastai.com) to
better understand the state of the art of machine learning. The goal
of the course is to bring more people into the field of machine
learning and fast.ai's library lowers the barrier to entry into the
field. The first class really showed how easy it is to get good
results using the tools that people in the field have built.

Deep learning is for everyone.
1. You don't need to understand a lot of math.
1. You don't need a lot of data. Sometimes <50 items is sufficient, depending on the context and application.
1. You don't need lots of expensive computers.

# Learnings

1. Underestimating the constraints and overestimating the capabilities of deep learning may lead to poor results
1. Overestimating the constraints and underestimating the capabilities of deep learning may lead prevent you from exploring solvable problems
1. Design a process through which you can find the specific capabilities and constraints related to your particular problem

# High level process

1. Make sure you have some data before starting, but don't wait for perfect or complete data.
1. Start with setting up an end-to-end pipeline.
1. Set up your pipeline for *fast iterations*. Do things like reducing
   your training data size, downsample images, etc.
1. Integrate and deploy early and often so that you can test your assumptions.

# Homework

In the first class, we were asked to build a simple visual classifier
by fine tuning a pre-trained classifier. Using the fast.ai library and
readily available images, I was quickly able to create a classifier
that was able to distinguish between sweet and savory foods from a
picture. I then published that model to a hugging face space:
[sweetOrSavory](https://huggingface.co/spaces/leftbyte/sweetOrSavory)

I trained this with about ~500 images of sweet and savory foods I
collected from a web search using these search terms:

```
sweet_foods = ['cakes', 'cookies', 'ice cream', 'pies', 'puddings', 'candy', 'soda', 'dessert', 'fruit']
savory_foods = ['dinner food', 'lunch food', 'sandwiches', 'pizza', 'casseroles', 'chips', 'crisps', 'fast food', 'pasta meals']
```

I started with the resnet18 model with a 20:80 validation/training
split.  The fine tuning took about 5 minutes and resulted in ~95%
accuracy. It was also very simple to export and load up the model into
an app on HuggingFace. Overall, this first dip into fast.ai's way of
doing machine learning was very fun and rewarding.
