+++
title = "On Prefabric #2, Halfway There"
date = 2018-02-24
+++

It has been more than one year since my last entry, and even more since the first commit of Prefabric. I neglected noting things down for a while due to major changes in my life (like moving to a new country), but the flights back home gave me enough boredom to put this wall of text together.

Let's go.

### Immersion lost through dimension reduction

Now that sounded rad, eh. Well... One of the leading hooks of Fabric was the concept of mutable space. Our brains are so hardwired to perceive what's around us as fixed and constant, the power to change the shape of the environment alone gives that wow effect pretty good. This works, because we witness this event from a first person perspective, one we're extremely familiar with. Even the smallest poke to this perception leads to a nice contrast. If change our point of view to a more, uh, non-familiar one, like "top-down", we don't feel that much of a contrast. Even though the exact same thing takes place, it's not "what happened. what is this place. where am I." but only "some cubes disappeared. so what." since the player is not in the game world anymore but only an observer from outside.

Of course this has also ties to the game taking place in a much more casual environment than Fabric, where both the hardware and the player's state of mind isn't prepared to give / receive a full first person experience. But I suspect a possible Steam version of Prefabric will suffer from this loss of immersion.

### Stackless challenge

Bending mechanic facilitates a stack. It is a "first in - last out" concept; the players can unbend only the last bend they did. In some levels of Fabric, we had used the nature of bending from this perspective where the players have to be aware of the stack's current state. In those places, they can only proceed with a very certain configuration of the stack. This is challenging for them, because a mistake in keeping track of the stack, namely an accidental unbend or a wrong order of bends, takes a train of thought in a conscious level to figure out. Since the time cost of constructing the stack, which is running around and shooting red cubes, doesn't feel that engaging, the players accept the intended challenge and center their thoughts around the stack. Therefore we had an additional way of making fun challenges.

Unfortunately though, we don't have that in Prefabric, due to the fact that bending and unbending are way easier for the players to play around. Since the game is top-down, they can just tap on the tiles they want. Whereas in Fabric, their field of view, walls, gaps and sometimes gravity itself obstruct them from doing whatever they want to achieve. Therefore we don't possess a very valuable tool, lost in the name of making the mechanic more accessible.

![prefabric_ss_2](images/prefabric_ss_2-1024x579.png)

### Only one Monobehaviour?

Probably most of the Unity programmers had hit this road block at some point on the way of learning Unity. Since Monobehaviours manager their own lifetime with Start() and Awake(), hidden dependencies between them create nasty couplings in the long run, which get very difficult to resolve after many workhours are done with those dependencies in place. To avoid these kind of issues I had [decided](http://atilkockar.com/prefabric-devlog-1/) to stay away from Monobehaviours as much as possible.

But that doesn't mean we should have one and only one Monobehaviour in total. For instance, raycast collisions on gameObjects are much easier with a Monobehaviour attached to them. Furthermore, an object which has to know references to many gameObjects, for instance a UI window, is better off with a Monobehaviour on it. Because the alternatives, somehow passing more than a dozen gameObjects in the constructor, or worse, finding them via name matching, is less clear to understand and harder to maintain.

However I did get away with using only one Start() per scene, using some sort of Init() functions that is manually called for every other initialization case. This way I set every piece of data at my will. Same went for Update()'s. Having only one Monobehaviour.Update() which calls other updates visualize what's happening through the frame. That's a mindset I'm going to further take advantage of in projects to come.

### Squeezing mechanics

One of the worse (at the same time, better) parts of Fabric was level design. I had talked about it in detail in [this article](https://www.gamasutra.com/blogs/AtilKockar/20161026/284098/On_Puzzle_Design_and_Fabric.php), but the gist is that solving the puzzles doesn't give that feel of satisfying click. One particular reason is worth further elaboration here: Although the mechanic itself is pretty solid, facilitation of it is left considerably weak.

In Fabric we had four different mechanics that work in harmony with bending. In the name of producing as many puzzles as possible with each of these mechanics, we had omitted the evaluation of these in a necessarily deeper level. Our focus became quantity over quality - with the exception of the tutorial / first act. We had so many features to explore and so many puzzles to build, we didn't stop and think about the contexts in which the mechanic can be used in such a way that the players would construct a train of thought (which is designed by the devs) and witness that it actually works. In other words, after making a few pretty basic puzzles (at best, at worst it was frustrating trial-and-error experiences) from a mechanic usage perspective, we left the mechanics to their fate.

Therefore, one of the aims of this project became to approach one single mechanic from as many angles as possible. Currently I have only one mechanic in Prefabric and it is going to stay that way. Thus, I'll go through the experience of expanding the mechanic even when it seems with a fair certainty that there are no more ways to assemble a fun challenge with that. So I'll be learning how to squeeze a mechanic even if that's all there's to be done. I think we all can agree on that restrictions are the main fuel of creativity.

### Less is more

We had certainly bit off more than we can chew in Fabric. Although I can safely say (for myself) that I did my best at the time, a whole lot of mistakes and lessons learned from them are what is left with us at the moment. Lessons that we are certain about making Fabric 2 way better than the first one ever was. Sure, it gives a good amount of eagerness to set things right in the next try, but we refrained from doing that. Those lessons are best tested on a much smaller scope, a mobile puzzle game for instance. This way the experience can be tweaked and enhanced much easier, since its bounds are considerably narrow. Furthermore, in a case of possible failure (which is going to occur, one way or another), total loss will not be in a scale we will mourn after.

Make one and only one thing, and make it good.

### Separate level editor

Both Fabric and Prefabric facilitate voxel-based levels. Since the experience is centered around these levels, it was evident from the beginning that we were to spend a considerable amount of time building and editing levels. That is the main reason we care about the level editor so much. It is almost a separate game with its codebase and built executable etc.

Having a separate level editor besides the game itself solves the problem of letting the players constructing their own levels. If we do not intend to establish and maintain an environment that supports user levels, making the level editor user-friendly and keeping it stable becomes an unnecessary chunk of workhours. On top of that, Unity itself already provides basic functionality like displacement gizmos and undo-redo stack. We have to have good reasons to re-implement maintain those features.

For the next project I'll work on, from the beginning I'll make the decision of whether or not bundling a level editor with the game on release, and if the answer is no, I'll just carry on with XML exporting Unity Editor scripts.

![prefabric_ss_1](images/prefabric_ss_1-1024x579.png)

### Replayability through restriction

Prefabric is a short game. On the avarage, I estimate a good part of an hour to beat until the end of the last level, not more. Though, making it longer would be a serious effort due to the fact that the whole game is built upon a one single mechanic. Eventually, good-feeling levels will more and more difficult to think of, and "that's enough I gotta release this thing" dread will settle in (like it did not yet...)

The only feasible option to make the game more lengthy without adding content, is to reuse the existing content in some way. But how would the players would want to replay a level, re-solve a puzzle they've already solved? Achievements of course!

I'm thinking of exploiting that completionist mindset a little here. There'll be seemingly useless visual elements, like that huge QR code in the level selection UI, which are going to stand out in a minimalist atmosphere, suggesting the players that there is more than what they see. In addition, there'll be carefully worded a couple of achievements to further scratch the players' curiosity.

All of those are going to point a second, more restricting playthrough. This playthrough will consist of the same levels, but to pass each of them, not only the players must reach the exit, but also they will have to provide the most optimal solution. It may not look like it at the first glance, but that's way more difficult than just trying to solve the puzzles. Even the smallest mistake will cost the players the whole level. That might sound a little frustrating, but in fact it's fine because the players who got to this point most probably have the perseverance to handle that, they will be looking for a challenge.

This is also going to be very close to the experience in my mind, which involves carefully thinking and planning the moves before executing them. We've had enough brute force players back in Fabric.

### Taking notes

One last thing I want to mention is the importance of being able to take notes at any given moment throughout the day. Many decisions I made about Prefabric, including all the points I made here in this article, came to me while walking to U-Bahn, at the grocery, while doing the dishes (or other places that I don't need to mention. You know them very well). As soon as the thought occurred to me, I noted it down no matter where I was. Because living through our ordinary lives succeeds at making us forget those points very well. I learned not to trust my memory and noted everything down. And I suggest you to do the same. Google Keep on my cellphone helped me immensely here. Highly recommended.

![prefabric_ss_3](images/prefabric_ss_3-1024x579.png)

\_\_\_\_\_\_\_ I guess, with the given tempo of these blogposts, the next one will be a Prefabric postmortem. I honestly suggest you not to expect to see it before a good few months. But who knows, maybe I'll finally get to work on my... well... I'll talk about it when it's ready.
