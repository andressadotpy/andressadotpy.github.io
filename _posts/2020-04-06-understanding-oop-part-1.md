---
layout: post
title: Thinking OOP - Part 1
tags: [OOP, OOD, Object-Oriented Programming, Object-Oriented Design, Python]
---



# Thinking OOP - Part 1

  

It's been a while since my last post and just now, as things in the whole world are very weird, I'm back. So, **#StayAtHome** and keep up with me! Lately, I'm deepening my knowledge in Object-Oriented programming and reviewing some concepts about this paradigm. So, I decided to make a series of posts about OOP and *how to think* in an Object-Oriented manner. In this first part I will be talking about some fundamental concepts that will help us design better Object-Oriented Software, such as *Abstraction*, *Encapsulation* and *Information Hiding*. Also, I explain **what is an object** and why choose the OO approach.

I'm using as bibliography [Python3 - Object-Oriented Programming](https://www.amazon.com/Python-3-Object-Oriented-Programming/dp/1849511268) and [Designing Object-Oriented Software](https://www.amazon.com.br/Designing-Object-Oriented-Software-Rebecca-Wirfs-Brock/dp/0136298257).  Both are very good books that I recommend reading. If you are studying Python, *Python3 - Object-Oriented Programming* is an amazing book to read after having some basic knowledge about the language. I read it twice already and always use it when I need to remember something.    



## Why Object-Oriented and what is Abstraction

  

As developers we are always finding ways to make our applications more **reliable**. It's not different for us when we choose an Object-Oriented approach. Software applications are big and complex, but they are not static. During its life cycle, Softwares are always being extended and modified, and we need to find a way to make these changes with **as minimal side-effects as possible**, and OOP does that. An **Object-Oriented Design intents for Software that can be easily reused, refined, tested, maintained and extended** and therefore *increases the application's life cycle and its reliability*.  

> "Abstraction is the key to designing good software."  

I believe **abstraction** is the most important concept in this post and is our guide to understand everything that will follow. Abstraction allow us to deal with different levels of details to a given task. For example, a *computer user* needs to deal with a different set of details that of a *computer engineer*. A computer user doesn't need to know information about every piece inside its computer, but the engineer does. **The computer user and the computer engineer deal with different interfaces because they have different intentions for the computer**. In this example, we can identify the computer as an **object**, with two very different interfaces.   

The use of abstraction makes it possible to model very complex problems. If we want a map from the city routes, for example, we don't need any more details than just the routes. Everything that isn't essential for the map is not in the map and all the important information is. In other words, the purpose is to abstract out information and *encapsulate it within objects*. Objects are the basic components of the design.  



## Encapsulation and Objects



I was thinking of a way to divide these topics and explain them separately, but as they are both related, I thought better and decided to talk about both of them together. An **object** is a collection of **information** *encapsulated*. This information is **data and their associated behaviors**. Data is what describes an object and the behavior are functions/methods associated with each object.  

So, I like to think of Object-Oriented programming as a bunch of different objects interacting with others. Knowing the intention of the program makes it easier to find the objects we are dealing with and how they *relate* with the others. After identifying the objects, the Object-Oriented Design structures responsibilities for each object by asking "what does this object **do**?" and "what is the **data** that this object holds?".  

We are used to think of an object as a *touchable thing*. Let's think about a *TV remote controller*. There are very different TV remote controllers in the world, with **different weight, colors and sizes**. Some of them, maybe have a few more buttons than the others (a Netflix button, for example) and have more *functionality* than the others. But all of them are TV remote controllers. Their different weight, colors and sizes are their data attributes, that can be different for each one of them, but they all have this set of characteristics. Their similar functionality, changing TV channels, is a TV remote controller *general behavior*, that means it applies for all TV remote controller objects. *Each TV remote controller is a different object*, with different values for its data attributes and maybe extending or not the general changing channels behavior, but they are all grouped as TV remote controllers. Even though it's different when modeling computer programs, the thinking process is exactly the same.

> "Object-Oriented programming initially starts with a more abstract focus. It asks first about the **intent** of the program: the *what* and not the *how*. It goals are to find  the objects and their connections. (...)"

In a way, it's easier to identify the *objects* during the analysis as the **nouns** of the program. The *methods* of an object usually represents *actions*: they are the **verbs**. And the *data* usually describes the objects and can be nouns, adjectives, etc.  

As told before, the information is kept inside an object as a *capsule* and it can be hidden from external view. The object has both a **public** and a **private** faces. The public interface shows only how other entities can interact with the object, not how the implementation works. The private side of the object, which is how the object performs the operation or compute the information, doesn't matter for the rest of the program. The process of hiding implementation increases abstraction and makes it easier to change the private side of an object without interfering in the public side. This process is known as *Information Hiding*.



### Conclusion 

  

This post was very theoretical and full of concepts but I do believe that understanding all of this is essential to create well written Classes. Here we talked a lot about how is the process of thinking an Object-Oriented Software. We didn't write any code (yet), but I hope that when you were reading, you were thinking about how to improve your applications and apply this concepts better when writing a Software.  

In the next posts I will be talking about **relationship** between objects (Inheritance, Composition, Association and Aggregation) with examples in Python.