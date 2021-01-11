+++
title = "On Fabric 2: A Break"
date = 2019-12-18
+++

It has been a long time since the last entry, in which I had told that it'd be the case. In this one, I'll try to go through what has happened in the meantime.

## The Missing Question

Yes, that next step that follows having an idea to realize… I've told you about the 'why' question and its importance. I must admit though. Even after writing those lines, I made the mistake of not heeding my own advice, and didn't ask it myself why and write the answer down. After I was fairly into development and a certain amount of effort had gone into it, I was kinda afraid that the answer wouldn't make sense, or worse, would be wrong. All those thought and development work would lose its meaning. So I hesitated. Now that what's done is done and I'm past a certain checkpoint in production, it feels safer to look back and be bolder.

I started making Fabric 2 because I simply had to.

I checked back the Trello board that I've kept for the ideas and references etc. The first card I've created dates back to 16th September 2016. That's one month after Fabric's release. There are more than three years from that card until the 7DFPS jam that kicked off the development work. Throughout these years I always had been thinking about the mistakes we did and how we could improve the experience. After all, [as I've said before](https://www.gamasutra.com/blogs/AtilKockar/20161026/284098/On_Puzzle_Design_and_Fabric.php), that's all I could focus on following the release. The fact the mechanic is kind of original (the 3D version of it. I love you guys at [Nitrome](http://www.nitrome.com/games/faultline/)) supported the idea of exploring it further. It felt like we (torreng) are the ones who could give it what it deserves. Although many of them ended up in the trash bin, I had a lot of ideas and notes on how to highlight the mechanic through level design and a way of storytelling that I can pull off. Trashing many made the remaining ones even more precious. The experience wasn't very clear, but I had a list of things to try, and I had a strong feeling that they were the correct ones. Then 7DFPS was announced and I had no other choice but to start development.

Don't do the mistake. Don't let yourself go adrift with the excitement. Write down the purpose why you're doing what you're doing. And, most critically, be convinced by the answer.

![](image-1-1024x577.png)

If you can see the fireballs coming at you, then you're in the target audience

## Where We Are

If you've been reading the previous entries, it shouldn't be a surprise to you that the game is in a barely-alpha state. I knew that I'd be very tired of working on Fabric 2, due to it being a spare time 'project', and I had to give a break somewhere in order not to repeat what we've done with the first game. I had designed this milestone to contain all ideas I had in mind, even only the rough versions of them, so that it'll stand through an undefined length of break and I won't keep thinking about the general direction anymore when not working on it. We will see if that is to be the case.

About the content. The levels should take up to 2, maybe 3 hours of play time, excluding collectibles (and one special level that could take an hour on its own, unless you're good with… well… I won't spoil). There are a couple of levels for basic introduction for each mechanic and a couple more for the combinations, leading up to something around a total of 15 levels with interlude levels. I think they are enough to give the player a glimpse of what bending and gravity shift can lead to, in spite of being far from being optimal and polished. Combined with the occasional graphical glitch and lack of testing, the whole thing feels very much 'pre-alpha'

I had started with a mindset of 'we've done this before, this is how it works' and pretty much copied and pasted the introduction levels from Fabric (the introduction levels for each mechanic). A good amount of level design work went into these levels, which takes up the majority of the current content. The problem with them is that they feel, well, introductory. There's little challenge involved in terms of the mindset I want the player to achieve. They still wouldn't consider the space as a part of the puzzle and concentrate on the bend tiles again. I mean, that is to be expected. I copied the puzzles, didn't I.

![Image](https://pbs.twimg.com/media/EGOSMX3WsAIC3dk?format=jpg&name=large)

Rush B

The part where it started to shift was the moment when I took something that exists, for example E1M1 or Dust 2, and made a simple bending puzzle out of it (Thanks to [SabreCSG](https://github.com/sabresaurus/SabreCSG), they were rather easy to convert into Fabric levels while keeping the layout and the scale intact). The idea is still untested, so I'm not sure how well it works, though as far as I am concerned, they ended up being a lot more interesting to play than others. You get the sense of space being mutated rather strong. Furthermore, they were actually fun to build as well, since these are the places I've spent a lot of time within. Combined with the minimalist coat of paint, the contrast between the familiarity of the layout and the strangeness of the gameplay might provide a unique experience. It is painstakingly obvious, to me, that this is the area where the idea should be expanded upon. I had a vague belief in the beginning of the process that this would be the case, however the mental energy spent into the introductory content was so much that I was very eager to reach the milestone and be done with the whole thing. As a result, the interesting part is rather slim in the current build.

There was also the matter of strafe-jumping ability that we have. I had built the [player controller](https://github.com/atil/fpscontroller) way before this project had started, with a totally different purpose in mind. So when I put this into a puzzle game where you don't have to hurry in any way, it kinda feels out of place, as far as the puzzles themselves are concerned. I insisted on making strafe-jumping a part of the game, while not breaking the experience I've been thinking about for so long, because, well, I _had_ the ability. And, well, I like strafe-jumping. The answer on this, which I was able to reach the quickest, was the collectibles. There are, I think, like 10-11 of them in total, which are placed in such a way that either you know how strafe-jump works or a little more curious than the average player to reach them. It'll work OK in the end, I suppose. At the moment strafe-jumping and bending live in their mutually excluded spaces, due to the fact that the first iteration to merge them failing horribly. I'm hopeful though, there had been only one attempt in one level so far. Fabric 2 might eventually become a puzzle game for Quake players, if it is to become _anything_. I'd actually push for that. I remember feeling dizzy and not being able to work anymore for the evening, because I kept fine-tuning a particular section of strafe-jumping a little too long.

I'll share a link to the build here when it's available. The only piece of work remaining is the setup of the store page. Should be ready in a few weeks, at the latest.

## The Break

Here we are at the personal part of the post. By now, you should have an idea what happens when you don't answer the question in the beginning. Let me lay it down in clearer terms: There is no destination which this project is headed towards. Since this isn't a tech exercise sort of thing (which Prefabric [was](http://atilkockar.com/on-prefabric-3-why-and-answer/)), it feels like the product, or the experience, should be the goal that I strive to achieve. However, as I pointed out earlier, I still can't figure out how this mechanic can be a part of 'I must have this' kind of experience. I think this is because of the way bending changes the environment and how it is difficult to visualize what the outcome will look like. In most puzzle games, when you as the player do what you can do as a game verb, you know what's gonna happen and this is the exact reason why you do what you do. You intend to make use of what change that verb causes in the game world, and you know that _before_ doing it. Even though the puzzle is not, the verbs are transparent. In my case with this game, there is no practical way to do such a prediction, and the main reason is the inability of human brain to think in this pseudo-4d-space. It is interesting to witness, yes, but not fun as a puzzle.

Though, the real problematic part for me has become to come up with something finished. Let alone selling that, trying to obtain that finished product requires a good amount of sacrifice in terms of the overall life quality. You can read a lot about that on the net so I won't repeat any of that here. However the point for me is that I have to convince myself that the effort is going to worth it. I can wade through the drudge work very well if I deem the effort worthy and I'll tell myself 'yeah it sucked from time to time, but this is what I wanted to end up with' in the end. This conviction is directly related to the 'why' question, though. You might imagine how difficult it would be to keep pushing for a product that you don't believe in, in the time frame where you can play Quake.

I'm writing this entry around the time of publishing the milestone that I'd think I'd reach and then give a break. I'm experienced in developing side projects and I had seen that coming even before kicking off the development. The thing is, I don't know if I want to continue at this point, due to the reason I stated above. This might change in the future, but I don't want to think about it just now. I want to give a break. Not just to Fabric, but to doing 'projects' in general. I don't remember not having a side project since I learned Unity, which was around the release of 2.6 version. Every single day I come home from work, I feel the responsibility of working on something of my own, even if that piece of work isn't my favorite task but has to be done in order to have a finished product. This is gonna end, temporarily.

![](image-1024x578.png)

\*DOOT\*

## Next.

Last weekend was spent with tidying up the backlog of game ideas. Yes, it took two full days. I used to keep a note in Google Keep; a list of one or two word phrases. I migrated that list to OneNote (because it provides a way better format for the bookkeeping I need) and went over every single entry, no matter how silly or impossible it sounded, and expanded the idea by doing some brainstorm. The highlight of the work was the 'why' question. I asked 'why would I want to make this game' for each one of them, and wrote the answer down. To be honest, that wasn't very helpful to find out what I actually want, however I eliminated many items, some of them were in there for a long time.

I have picked one to follow in the months and years to come; _the_ one game I always wanted to make. In fact, I want to _play_ it instead of making it, but I've been waiting for a long time for someone to make this sort of a game. There has been some tangents to the idea, but so far no one was able to pinpoint that. So that's a first for me: I have an experience to realize now. This is not _any_ experience though; it has been cooking in the back of my mind for a _very_ long time and it will take _even longer_ to come up with something that people can interact with and have fun doing so.

But it's fine, that's not the purpose. This time, the purpose will be the joy of creation itself.

Sure as _hell_, I'll be _dying_ to show you the _fire_ I'll be feeding.

\_\_\_\_\_\_\_

Honestly I don't know when the next update is going to hit. I might not have anything to share for an indefinite amount of time. Expect a long break to posts.