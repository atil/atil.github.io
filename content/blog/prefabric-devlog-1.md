+++
title = "On Prefabric #1, Initial Commit"
date = 2016-12-10
+++

Sorry for the wall of text. I guess I had just too much to say.

Edit: I forgot to mention that you can browse the source on [Github](https://github.com/atil/prefabric)

This is the first time I talk about my new pet project, Prefabric. Following the release of [Fabric](http://store.steampowered.com/app/489660), I finally had the chance to think thoroughly on technical and design issues of the game, [as I've pointed out](http://www.gamasutra.com/blogs/AtilKockar/20161026/284098/On_Puzzle_Design_and_Fabric.php), and it's not even the complete list. It seemed like there were so many things done wrong that I felt so eager to jump into a new project to do those things right. I left Fabric so behind that even though I'm able to get my hands on a VR headset and know how awesome it could be, I'm not working on a VR port. Hell, I didn't even start on Mac/Linux builds, which are roughly one click away. This is a mistake I'm currently doing, and I'm well aware of it. I hope I won't regret it too much.

### The Idea

Anyway, I should be talking about Prefabric. The idea is stolen from [my colleague](http://barankahyaoglu.com/dev/), who stole the idea from Fabric. In Fabric, it was hell of a nightmare for us to make players establish a sense of location. When looked from afar, it's much more easier to see what's going on. He made a prototype to show this (and he was right, this angle is way less of a hassle for players to overcome) which is unfortunately isn't publicly available, and left it to its fate. Recently, I had to do a Unity project for my master's degree, and decided that school deadlines would come in handy for working on such a project. You know, you need them for your pet-projects. Besides, just like Fabric, the core concept is worth investing time into.

![prefabric_3](images/prefabric_3.gif)

During the development of Fabric (and following the release), I've improved my software engineering skills, thanks to my day job. I've seen many tools and patterns to solve problems, which can be encountered in development of games of any scope. With Prefabric, I'm planning to giving them a shot. These may sound (or prove) unnecessary, wrong or downright ridiculous. Still, I see much benefit in trying new tools on expendable projects, experiencing their goods, bads and uglies first hand.

### The MonoBehaviours

Are no longer... The main reason I'll try to leave them as much behind as possible is my limited control over their life-cycle. Awake(), Start(), OnEnable(), OnLevelWasLoaded()... All are used for initiating objects. You begin with Start(). Then you need some stuff done before Start(), you add Awake(). Actually, this point alone causes many hidden dependencies. As the project grows code-wise, the initialization becomes a mess, unless we knowingly, actively do something to prevent it. I'm not even mentioning the randomness between Awake() / Start() calls themselves. Your game is working good in the editor, you build it, and executable fails? You checked everything and it actually should be working OK? Well joke's on you, some Start() decided to be called in a different order. And because your dependencies are screwed up, unbeknownst to you, that Start() turns out to sit in an important chair. And on top of all those, guess what? You had some code to run on some object's OnEnable() because you need stuff done on some "enable = true", and the party is just getting started.

These all are trivial errors to solve by themselves alone, but I think they shouldn't exist in the first place. I feel way more comfortable when the code runs at _my_ will, not in a time frame decided by Unity. I plan to use just a single Start() and Update() located at a central place, unless I absolutely need to have another one.

Another reason is performance. I know, for Prefabric this indeed isn't an issue, since there are just a handful of objects. But as mentioned before, I want to know this approach inside-out. So that in a future project where I'll have to squeeze every bit of performance from Unity, I'll have a little bit of idea about what I'm doing.

### The Messaging System

Deciding on dependencies should be given a considerable amount of thought. Which module knows which one, what is this particular module's responsibility, what's gonna be the interface between these two modules and questions like this have the utmost importance in terms of modularity and writing readable code. In the class hierarchy tree, sometimes you find that two guys at the opposite ends of the chart should be talking to each other. This may itself be a result of bad design, I don't know. What I know is, unless you put a shortcut between those two, carrying a message through more than a handful of irrelevant classes definitely leads to "wtf" moments.

To loosen this dependency tangle, I'll be using [UniRx's MessageBroker](https://github.com/neuecc/UniRx/blob/0f4659c201062e1da37e86f3cb23aeddcedd1e6c/Assets/Plugins/UniRx/Scripts/Notifiers/MessageBroker.cs), which is basically a global event-subscription system. Any message a module thinks other modules should know about, is published as a custom class, say "BendEvent". Any class who wants to know about bending subscribes to _type_ "BendEvent", which is basically giving a callback function to that event. The rest is up to MessageBroker.

The good of this approach is, like I said, there isn't any complicated dependency graph. Any class can know about any event happened in anywhere of the code. On the other hand, I'm not sure if this is a good thing but, removing a class doesn't break other classes.

The down side may be that relying too much on events, or an increase in their numbers, can lead to execution order complications. "This event should run, but it waits for that other event, which is not thrown because blah.", for instance. I need to be on my guard for any thoughtless message chain from taking place. [KISS](https://en.wikipedia.org/wiki/KISS_principle), remember? I can't be sure of it though, since this is my first attempt in using such a solution. This may backfire horribly as well. We'll see together.

### The Commands

OK, this definitely is an overkill for Prefabric. I'll explain that in a second. There is a class "ControllerBase" and the comment above reads something like "command emitter". So here's my train of thought on the issue: There are agents in the world (player, bots, enemies etc.) which need to take commands (move, jump etc.) from _somewhere_. The idea is that the _Move_ command is the entry point from _somewhere_  to the game. It doesn't matter where it came from, the agent will _Move_. That somewhere can be your keyboard-mouse, gamepad, touch screen besides many others, if there is a human in question. If the game is multiplayer, the networking component will emit _Move_. Or if there are bots, A.I. component will be doing so. If you are playing a recorded demo, a FileStream will be providing commands. You get the idea.

Command emitting business seemed like a sensible thing to abstract away, because the use cases actually are from scenarios that are very likely to happen.

Now, as to reason why this being an ovekill... Because those scenarios aren't going to happen. This is a small school project, and I try to outline the scope as narrow as possible. I'm pouring sweat on this matter purely because of practicing it.

### The Level Editor

This is something I learned when developing Fabric. In both that game and Prefabric, gameplay revolves around levels for a huge amount. In other words, levels being good means that the game is good. From the very beginning of Prefabric, it's very obvious that I'll have to spend a ton of time building and tweaking levels, maybe as much time as the development itself, if not more. For this reason, I've begun putting together a usable level editor before a playable game. Up until the release, I'll see to that it'll suit _my_ needs. After release I'll try to build a level sharing platform to see what other people will come up with. We've been said so many times that a Steam Workshop feature would make Fabric a much richer experience. I guess I don't need to emphasize what modding communities do to games these days.

Starting with writing a .json file reminded me this, by the way:

https://twitter.com/InternDept/status/745680279424008194

That's it I guess. These were roughly the things my mind has been wandering about lately. As someone experienced with pet projects, I know how much commitment these things require. So I'm trying to follow that famous "do something every day, no matter how little" approach. As soon as something to do comes to mind about Prefabric, I immediately write it on [Trello](https://trello.com/b/vvbPBpeb/prefabric). That way my brain gets rid of "deciding on what to do"overhead and as soon as I open up Unity, I can start working. Prefabric is coming up slowly but steadily.