---
layout: post
title: An Introduction to Topological Data Analysis
---

TLDR: Topological Data Analysis uses abstract maths to uncover hard to spot patterns in high dimensional data. You can use Python to get your hands dirty with some computations but first it helps to understand some theory to know what you're actually looking at.

In this blog I want to touch on a recent exciting intersection of pure mathematics and data analysis, hoping to be able to explain the concepts and techniques involved to those who do not have a masters in pure mathematics. 

In 2018 coming to the end of my degree I was excited to read that the very abstract area of my pure mathematics masters had an application in data science in the area of [Topological Data Analysis](https://en.wikipedia.org/wiki/Topological_data_analysis).

What is Topological Data Analysis (TDA) and why do I care? TDA aims to apply techniques used to study the shapes of abstract spaces in mathematics to find shapes and patterns in data that we otherwise would not be able to see. It has been used over the last decade in areas such as medicine to identify patterns previously missed such as a [subgroup of breast cancers](https://www.pnas.org/content/108/17/7265).

## The Maths
Lets start with the area of maths thats used in TDA: algebraic topology. The main concept we need to care about is that of the 'Homology groups of a space'. 

### The Group Theory
Very roughly a group is just a collection of elements which confirm to a certain set of properties and can be combined using certian rules. The best example is the integers, which we will denote by $$\Z = \{\dots,-2,-1,0,2,1\dots\}$$, this set of numbers forms a group as you can add two integers back and get another integer, and you can subtract them and get another integer and you have the special element $$0$$. We would say that '$$(\Z,+)$$ forms a group under addition'.  For the purpose of this post we do not really need to know what a group is in any more detail.

### The Topology
Topologists decided some while ago that we can take all of the fancy mathematics that we have developed to study the theory of groups, and try and apply it to study the theory of spaces. They cared about how we can tell if two spaces are 'the same' (under certain topological abstractions). For example if I give you the outline of a circle and the outline of an oval, we consider these to be the same as you can bend and stretch one into the other. They are both just lines with a hole in the middle. 

On the other hand if I give you the outline of a cirlce, and a solid sphere, these are fundamentally different. The circle has a hole, the sphere is solid and has none. This is where Algebraic Topology steps in, we can use group theory to formalise these differences and come up with a mathematical way to detect the 'number of $$m$$-dimensional holes in an $$n$$-dimensional space'. 

### The One Where The Group Theory And The Topology Met
Enter Algebraic Topology, we take a space, and assign to it a series of groups, called the homology groups of the space. We index these by a number so we have the $$m$$-th homology group of a space $$X$ which we denote by $$H_m(X)$$. This group tells us about the $$m$$-dimensional holes in our space $$X$$. For example a (hollow) circle has a hole in the middle we will call a '$$1$$-dimensional hole'. A (hollow) sphere, so imagine a football, has a $$2$$-dimensional hole in the middle of it, and so on. A hollow donut, encloses the same kind of space as a sphere does, but it also  has the same kind of hole that a circle does if you go both ways around it, so the torus has two $$1$$-dimensional holes, and one $$2$$-dimensional hole. Lets now turn the above intuition into maths notation. In each dimension we use a copy of $$\Z$$ to represent the presence of a hole, so $$\Z$$ means one hole, and $$\Z^2$$ means two holes, for the sake of this post don't worry about why this is (for those that know group theory, what we really have is isomorphisms between the integers and the homology groups).

For a (hollow) circle which we denote $$S^1$$:
$$H_1(S^1) = \Z$$,
$$H_n(S^1) = 0$$ for all $$n > 1$$.

For a (hollow) sphere which we denote $$S^2$$:
$$H_1(S^2) = 0$$,
$$H_2(S^2) = \Z$$,
$$H_n(S^2) = 0$$ for all $$n > 2$$.

For a (hollow) donut, a torus, which we denote $$T$$:
$$H_1(T) = \Z^2$$,
$$H_2(T) = \Z$$,
$$H_n(T) = 0$$ for all $$n > 2$$.

I understand a lot of the notation is hand waving, but the important concept to grasp is this **the m'th homology group of a space tells us about the m-dimensional holes in that space**.

## The Data Science
Now we have a grounding in basic algebraic topology, lets turn our attention to some data. Given a data set, initially all we have are a load of points scattered in space. In order to apply the above techniques we need to make these somehow more 'solid', enter the [Vietoris-Rips complex](https://en.wikipedia.org/wiki/Vietoris%E2%80%93Rips_complex)! What we do is pick a radius $$\epsilon$$ and look around each point within that radius, if there are other points close by lets join them up. Applying this accross our data set gives us a kind of space we denote $$R\_\epsilon$$. For each $$\epsilon$$ we can create our sequence of Homology groups $$H_1(R\_\epsilon), H_2(R\_\epsilon), \dots$$. 

TODO: include example of rips complex

As the $$\epsilon$$ increase in size different features become apparent in our data, and so parts of the homology may appear at one $$\epsilon_1$$ and dissapear at some $$\epsilon_2$$. For example consider a data set of a circle of points. When $$\epsilon$$ is small the space we form is a circle, however at some point $$\epsilon$$ will get so big that all of the points join into one to make a disk. Hence we see the $H_1$$ changes as epsilon increases, the $$\epsilon_1$$ where a feature first appears is called the 'birth' of that feature, and the $$\epsilon_2$$ where a feature dissapears is called the 'death' of that feature. We collect all of these paris of birth and death values together into a set and display them on a diagram called a 'persistence diagram'.

TODO: Include example of persistence diagram.

## Putting It Into Practice
So now we understand what Topological Data Analysis entails, how can we turn this into actual code to apply to data?! It turns out some very nice people have put together a set of packages in python to compute the Persistent Homology and Persistence Diagrams of a dataset: the [scikit-tda](https://scikit-tda.org/) python package. Just run 'pip3 install scikit-tda' (note you may need cython as a prerequisite). 

## Next Steps
Now you have seen some Topological Data Analysis in action, take your favorite data set and run it through the libraries from scikit-tda, to see what patterns may be revealed. The above is really just the base minimum you need from mathematics to understand some of the concepts here, there is lots I glossed over for simplicity and the interested reader has many tens of hours of abstract algebra, point set topology and homological algebra to read before the standard algebraic topology makes sense from the ground up. However in order to just apply TDA in practice much less of this is needed, and hopefully this post has been able to demistify some of that high barrier to entry.

If readers are interested in further exposition on TDA, or more on the mathematics behind TDA I am happy to write further posts, just shoot me a message (linkedIn is easiest)!

## Advanced Topics and Further Reading
Further group theory: [Warwick's Introduction To Abstract Algebra](https://homepages.warwick.ac.uk/~maseap/teaching/aa/aanotes.pdf)
Further algebraic topology: [Hatcher's Algebraic Topology](https://pi.math.cornell.edu/~hatcher/AT/AT.pdf) (note: this is the free copy published by Alan Hatcher himself)
Introduction to TDA (much more in depth than here): [Murugan et al, Arxiv Paper](https://arxiv.org/abs/1904.11044)
Example of an application of topological data analysis: [Gidea et al, Arxiv Paper](https://arxiv.org/abs/1703.04385)
