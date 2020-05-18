---
layout: post
title: Functional Programming For Finance - Part 1
---

The audience of this series of blogs is to introduce some functional programming ideas to those who work in finance, maybe as Quants or Quant Devs but have worked mainly with object oriented or scripting languagues such as Java or Python. Many people who come into finance out of PhDs or non computer science degrees may not have come accross the concepts involved in functional programming, but they are becoming increasingly prevalent both accross popular languages and in industry (e.g. Jane Street with oCaml and stream style programming in Java).

Definition: Functional programming is a programming paradigm where programs are constructed by applying and composing function. (Source: [wiki](https://en.wikipedia.org/wiki/Functional_programming))

We will be coding today in Haskell.

Lets start by installing Haskell:
- Follow the instructions [here](https://www.haskell.org/downloads/) to download the Haskell Platform
- Type 'ghci' in  your console to enter the haskell interactive prompt
