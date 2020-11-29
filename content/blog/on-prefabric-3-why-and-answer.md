+++
title = "On Prefabric #3, The 'Why?' and the Answer"
date = 2019-01-13
+++

I released a game, and I told no one about it.

It sort of feels wrong at the first glance, right? Having to work on an experience that you think is cool, for a considerable amount of time. Sacrificing the precious after-work hours in which you could play games, watch Netflix, solve the Rubik's cube, stare at the ceiling etc. Basically anything that doesn't include kicking the disciple switch on and make yourself feel a little bit more miserable for the evening. You think it's worth it, worth the pain, in the end. This is a marathon and you have to force yourself sometimes. You dream of that release button, followed up by positive and encouraging player comments on the bottom of the page, maybe some coverage on press websites. Of course you'd be sharing that store page pretty much everywhere, right? That's the result of your two year's hard labor. If you don't, why did you make the game, then?

That's the _exact_ question we should be asking ourselves way more frequently.

I've talked about Prefabric before. It started as a school project, so I never got the chance to ask, I had just begun building it. Throughout the development, certain things became clear by themselves, and I've told myself 'yeah, these things sort of emerged from nowhere, let them be the pillars, therefore the whole thing won't feel pointless'. Those matters I've delved into detail at length in [previous](http://atilkockar.com/prefabric-devlog-1/) [posts](http://atilkockar.com/on-prefabric-2-halfway-there/). When people told me 'hey don't do it this way, it won't sell' or _I_ have told myself 'I can make this code much simpler. There's no need for this much of thought to give', I have repeated those pillars again and again. They turned out the be the answer to the question I had to have asked in the beginning:

_Why am I making this game? What do I want to achieve?_

In fact, we all have the answers to these questions, deep inside. Of course there's _somewhere_ we wish to be, at the and of this marathon. Why else would we wade through what feels like a drudge work sometimes? You'd never say 'well, I dunno why'. But what we should do is to ask them out loud, and write down the answer. After that solid answer is there, you pretty much can decide on everything with a sharper certainty and spend your precious mental energy way more efficiently. From coding to level design, art direction to marketing, root of every course of action lies within that answer.

## What Prefabric Brought

In a rough way, with Prefabric my aim was to code a game in a pub-sub pattern, and to make the most out of the bending mechanic itself. Let's see how they turned out.

#### Experience with 'pub-sub'

Around the times the development has started, we were using this library called UniRx at the workplace, for some project. It felt like a great way to decouple components, which would lead to being able to implement plug-and-play features. The way of thinking also felt natural; you immediately can figure out the what 'events' are in the game world and what happens in reaction to those events. So I went ahead to try to build a game around that mindset.

Where this pattern really shined was how easy implementing new features had become. Most code to run were the reactions to either bend/unbend or player input, so it's not exaggeration to say that the whole game is built within those two callbacks, aside from the a single pair of Start()/Update(). It even went to a point where some objects are new'ed only once and never touched again, because all it needed came through those events.

This sort of flexibility has its price, however. There can be a number of roadblocks if you're not careful, nastiest of them being the call order of subscriptions. Now, it's all good when all subscriptions handle a completely separate part of the game, but when you find yourself in a situation where one subscription's work depends on another, you know you're in trouble. As a simplified example, I had TileClickCommand on top of which pretty much everything was built (I had talked about commands [here](http://atilkockar.com/prefabric-devlog-1/)). Movement and bending code run in subscriptions to this event. During tutorial implementation, there's this concept of invalid clicks came up. In tutorial, I wanted a very hand-holding corridor and very specific tiles and UI buttons to tap on. If a click isn't validated through the tutorial data, then it was to be ignored. This meant that all on-click logic had to be run after the tutorial subscription. The solution came up with was to separate these two concepts to TileClickCommand, the player command, and TileClickEvent which describes a valid tile click. Although I could stay within the logic-in-callback-only zone with this, there were other cases I couldn't. Those were mostly about Unity-specific things like coroutines being run at the end of the frame, gameObject activation etc.

##### ...and "pub-un-sub"

One other problem came from my lack of experience on event-driven systems. The name "pub-sub" deceptively hides one very crucial part of the solution: unsubscribing. In a simple implementation like Prefabric, pub-sub pattern's basic data structure is a mapping of event types to function arrays. Whenever a type is invoked by it's name, a series of functions are called sequentially. This operation happens in the static space, unbound from any sort of context. At least this is what happens within the countless abstraction layers of UniRx.

The first stone to the head had come early: Use a gameObject in a subscription, reload the level, publish the same event, and feel bad about that Unity-fied nullref message. Turns out that the gameObject is destroyed due to scene reload, but the subscription from the scene before is still there. So I went ahead and figured out that some events are meaningful only in a level, like bend event, and cleared out all subscriptions of these 'SceneEvent' types on a scene load. It worked fine for some time, until the second stone. The achievement module's subscription is also gone with a scene load, and unlike game scene subscriptions (which happen on scene load), I didn't bother to refresh them every time. Because in theory, achievements is a static-context matter and if I'd consider a red flag if I'd have to re-init it during the normal course of the game. My solution was to get rid of SceneEvent concept and put a guard gameObject to each subscription for null check instead of unsubscribing it. If that guard object is destroyed, don't go into the subscription itself. For move event, it was the player for example. This gave me the flexibility I looked for over the subscription's lifetime.

In hindsight, there were probably better solutions to the problems I've encountered. Finding the best or most efficient wasn't the point, however. Now I have a better understanding of this pattern, an insight to perceive potential problems before they appear in a future project.

#### Mechanic Restrictions

Another self-imposed restriction was about the usage of the core mechanic. We all know that everything is built upon the core mechanic; it is the very foundation of interaction that governs the _fun_. However, [as I've discussed before](http://atilkockar.com/on-prefabric-2-halfway-there/), getting lost in the excitement of side mechanics is a thing and results in under-utilized mechanics. What I mean with _under-utilization_ here is just showing off what the mechanic can do in a basic level, and not building up any kind of familiarity stair for the players to make use of.

##### Only Oranges?

I've named it "squeezing" before and I think it's an apt metaphor. Say that you're making a fruit juice and you have lemons, oranges and mandarin. You start the squeezing with oranges, but as you get tired and look at the remaining fruits, considering every one of them has a different method of squeezing, you sort of half-ass the oranges and move on to the lemons and try to figure out lemon-technique. The same goes for mandarin, and you end up trashing a great deal of fruit that still have some juice in them. In another scenario where you have only oranges, you'd have to use every edible bit to fill in the glass, because you don't have that many kinds of fruits anymore. Therefore, when you think there's no more juice to come out, you inspect the orange pulp in your hand again what you can do further about it. Furthermore, the knowledge you obtain will not be limited to oranges themselves. Since you know about them, the intricacies of the squeezing technique and the taste, you will know the exact aspects which will make them go well with the lemons. The decisions you give about the lemon usage will be more educated and calculated. The mixture will have a purpose, rather than being a random jumble of fruits.

##### Cleaner Level Design

One other result of this restriction was a more confined design of the general level progression. When I got used to the idea of thinking deep about a mechanic, the complexity of combining two different mechanics seemed to become tenfold or something. It felt too difficult to pull off properly, maybe more than it should. The more I got into the mindset of "do it well, or don't do it", the further I was from using two mechanics in a seemingly random way. I didn't feel like I would be able to extract the maximum amount of fun with a bend / 'mechanic X' combination, even though I knew that that wasn't the point. Therefore, it felt more doable to design levels which increase in difficulty in a sane manner, since I have a cleaner mental metric of being difficult.

## Closing Thoughts

Not every game is made with a stable income in mind. That's the luxury of side projects. They have the purpose whatever you give them.

Prefabric was not a project. Prefabric has been a practice and practices are meant to be disposable, [like Mike Acton have said](https://youtu.be/qWJpI2adCcs?t=3716). Now that it has done it's job, it's time to move on.

To Fabric 2.

\_\_\_\_\_\_\_

I've written a good deal about Prefabric up to now, but I now realized that I didn't include the guy who gave me the initial push to start this whole thing. Thanks [Baran](http://barankahyaoglu.com/dev/)!