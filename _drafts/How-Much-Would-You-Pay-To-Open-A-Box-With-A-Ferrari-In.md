---
layout: post
title:  How Much Would You Pay To Open A Box That Might Contain A Ferrari?
---

## The Setup
Suppose you go to your local airport, you see a stall selling keys to a mystery box which may contain a Ferrari, how much would you pay for such a key?

This is a standard interview question for quant roles, usually phrased like this: 'you have 4 boxes in front of you, one box contains \$100. After you open each box it stays open. What is the fair price of a ticket to this game?'.

## How Much Should You Pay
Lets start with the maths, to price the game we need the expected cost of playing to equal the cost of the prize, this gives us our 'theoretical fair price of a ticket' in which over many hundreds of games the seller and player would each win as much as they loose. So our equation to solve is setup as follows
$$Prize Cost = E\[Cost To Play\]$$.
If we work through this maths we see, in the case of a \$100 prize split over 4 boxes we get
$$100 = tP(win on first round) + 2tP(win on second round) + 3tP(win on third round) + 4tP(win on fourth round)$$
hence rearranging we can see the fair price is
$$t = \frac{100}{P(win on first round) + P(win on second round) + 3P(win on third round) + 4P(win on fourth round)} = 40$$
so in this basic case of 4 boxes with a \$100 prize the fair price of a ticket is $40.

Now if you are an enterprising store holder, you will need to make some profit margin, so you should charge $$(1 + profit margin)40$$ and on the other hand, if youre a smart gambler you should only play the game if the price of a ticket is less than the theoretical fair price of \$40.

## What About The Ferrari?
Assuming a Ferrari costs $250,000, and we have 100 boxes 

## The Code
We can code 

## Improving The Simulation
Our initial simulation has some unrealistic assumptions, in particular it assumes that everyone will always play the game to the end and has enough money to do so. In cases where the prize is very expensive this is certainly not realistic, also assuming a setup like a fair people come with a certain amount of money, and will spend some fraction of this per stall.

As such I have made the following more advanced version of the simulation which allows us to code a 'max_wallet' variable representing the most money a fair atendee will have. We then randomly generate per player:
- player wallet = a random amount up to the max wallet
- player max spend = a random fraction of this wallet that they are willing to spend on our game
A player will then stop playing the game if buying another ticket will take them over their max spend. This certianly makes the simulation more realistic, and for not much added complexity in code this is a fair improvement.
