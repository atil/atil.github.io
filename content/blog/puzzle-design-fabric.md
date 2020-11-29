+++
title = "On Puzzle Design and Fabric"
date = 2016-10-15
+++

It has been approximately 2 months since we released our puzzle game Fabric. So while probably it's too soon to write-up a postmortem, there are certain issues I'd like to talk about. I've been aware them during development, but I think calling a product 'finished' makes its issues much more eligible to be discussed.

For those who are like "Fabric? wut?", it is a first person puzzle game [on Steam](http://store.steampowered.com/app/489660), which also happens to be first (game) project I've ever completed.

In this post, I'll be addressing more on puzzle design side of Fabric. So here we go:

# ![](images/08.png)

# Puzzle Quality

What is a good puzzle? How can you say that a puzzle is well-designed? I don't know the answers to these questions. However there are a couple of things I experienced that may help you in your future endeavors.

## Solution Space

Solving puzzles are eventually about making decisions. Decisions like what action to perform, on what, from where etc.

Fabric's issue regarding to this starts right here. In about any puzzle game you can think of, the players 'construct' a decision by themselves. They are not offered a (small) number of choices to choose from. For instance in Portal, you can shoot about anywhere in a blank wall. Or in The Talos Principle, you technically are able to put the jammer anywhere in the world. You don't choose anything. You deliberately think of a position and execute your ability there. So, the actions you can perform are practically infinite.

Fabric offers the players a much much tighter solution space. In a level, there are a finite number of bend tiles and number of things you can do with them is even less, since they have to be axis-aligned.

So, when presented a puzzle to solve, the players naturally choose the path of least resistance: brute force. They don't try to solve the puzzle, because they actually are _able to_ try all the possible things they can, which eventually helps them progress.

To force to players to think, we have tried to add a ridiculous amount of fake bend tiles which doesn't have anything to do with the solution. It worked, the players _did_ think. Nevertheless, merely concealing the path from A to B doesn't necessarily produce a fun puzzle. Which brings me to the next section.

## Satisfying 'click'

Many people who played Fabric consider it as a good game, to say the very least. Frankly, I don't. Maybe it's because I worked on it so long that it lost its magic, like happens to all gamedevs out there, but I think the reason is something different.

I played Braid after releasing Fabric. (Yes, I know that it's a shame I've waited so long.) And I witnessed (pun intended) how the puzzles are so well made that I remember thinking "Oh this is reeeeaaally clever of you, Jon" many times. I won't go into detail how the mechanics are so successfully blended in together and all that. There is this thing I realized, however. In every puzzle, without exception, at some point solving it, the thought went through my mind: "Aaah so _this_ is how I solve it." The puzzles seemed obscure at the first sight, but after trying and failing a couple of times, the mask kinda drops and then it "clicks". You think of how the mechanics work and what are the implications, and most importantly how to bend them to achieve your goal.

This isn't the case in Fabric (at least for the most part). The players were figuring things out, but these "things" weren't related to the mechanics, which prevents them from feeling that satisfying click. The puzzles don't have that kind of transparency allowing players to acquire that mental boost from nature of bending / changing gravity etc. They were just doing whatever they are capable of and looking for some progress. Most of the reviews speak highly of Fabric, however they highlight the mechanics, not the puzzles.

Our (my?) approach to building puzzles was downright wrong. Just obscuring the path between point A and point B doesn't constitute a puzzle. Already, bending is somewhat unintuitive by forcing the players to think in ways that they've never done before. By the time they reach to point B, the chances are that they didn't accomplish anything with some train of thought which they have the complete control of. So they don't feel like they achieved something at all.

Making the players feel smarter in fact, servers a greater purpose: keeping them in flow. Being comfortable with mechanics can be considered as playing the game good. Since the concept of [psychological flow](https://en.wikipedia.org/wiki/Flow_(psychology)#Conditions_for_flow) is what we thrive to achieve as designers, we should be making the players feel as competent as possible. For puzzle games, this goes through making them understand properties of the mechanics and what they cause when executed.

![](images/01.png)

## Visual Progress

So this one was difficult to pull off for some first-time-devs like us. A should-be puzzle where you go from A to B, has an element indicating how close you are to solving the puzzle, or letting you know that you are on the right path. Whether it be seeing your physical distance to the goal is lesser, or arrows pointing at certain directions or what have you, this feeling of progress always uses the concept of space, and position of objects relative to each other. Because it's (conceptually) unambiguous and intuitive. You feel it pretty naturally.

This is the exact topic about which Fabric pulls the carpet under your feet.

How do you tell the player that "You're on the right path" when that path exists on some higher dimension of space? How can players feel instinctively that they are close to the solution, when distances are designed to deceive them?

Where we could, we put friendly looking 'checkpoint' tiles to (hopefully) make the players realize that they are on the right track. But believe me, those setups are very rare. In many cases, there's no single "logical" waypoint throughout the whole level, because we designed puzzles as a "whole", not in "here, then here, then over there" approach. As a result, the level of 'push effect of getting close to the solution' is zero to non-existent in Fabric.

* * *

So these are my concerns from a puzzle design perspective. There are others, about other faces of development like cutting content, Fabric's honesty and getting paid for the work I've done. I'll be discussing those in an upcoming post.
