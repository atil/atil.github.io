+++
title= "On FPS Game Progress: #1"
date= 2020-05-22
+++

I didn't expect to write another post for a much longer amount of time period. I don't know what happened, but I knew wasn't able to push it back anymore and started work on this, uh, tech demo. It _is_ going to be great one day. For now though, and upcoming months (maybe years) at least, it's gonna be nothing more than a tech demo / toy.

So let's move onto what it is:

https://youtu.be/TvgWOEnlXw4

As you can see, at this moment in time it doesn't look a lot more than a 'Unity tutorial part 2' scene. We have a character controller and we can walk and jump around. What's not immediately visible is the character controller logic, which allows Q3-style strafe jumping. Whatever it is I'm building, I want the controls to fit like a glove and melt away between the game and the player, so they must be super slick. Therefore I started from that part. Other than that, there isn't much to look at.

## Motivation

So as you can see, I'm building a game but… I'm not using a technology which I'm experienced at, which is guaranteed to make the development process shorter and easier. Actually I'm doing a whole bunch of things right in the opposite direction:

- Writing stuff from scratch with OpenGL
- Using vim with minimal config
- … in Rust

Well…

![](images/nn7jo.jpg)

But hear me out.

As I mentioned in the end of the [last post](http://atilkockar.com/on-fabric-2-a-break/), there's a very certain experience I want to realize. It's kind of difficult for me to explain in words (hence the game I'm building) but I'm laser focused on how it’s gonna feel like. [Everything in the game has to point in that direction](http://atilkockar.com/on-game-development-and-setting/).

The reason I'm not going for an existing technology for me is that there's already something there, with Unity for example, there's an environment with its shading, lighting, visuals, controls, etc. This is where my inexperience as a designer comes in: I haven't done this kind of thing before. I don't know how to approach this problem in a way that the end result would feel exactly I've imagined in the beginning. Very likely that at some point in time, I'll look at what I’d build and think "Hmm.. No this isn't quite right and _(here's the actual problem)_ I don't know what's wrong". It's like when you, an artistically illiterate person, look at a painting and you know it's not a great looking one but can't quite put your finger on why it doesn't look good. I'm not confident that I'll be able to figure out how to shape whatever Unity is providing me with and put it in _the_ direction. I feel that the easiest thing for me to start from absolute zero and place literally _everything_ with a consideration of contribution to the game feel. Therefore I'll end up with exactly what I wanted _and nothing more_.

So you see the mindset here, right? _Having only what you need and nothing more._

If you're a developer, you probably know how this is _so_ [not the case](https://tonsky.me/blog/disenchantment/) in the industry. This whole concept is a topic for another post, but I felt an alignment on this way of developing software and how I wanted to shape the experience: Add it only if you need it.

Specific to vim: I wanted to be on the keyboard (and the home row) as much as possible for ergonomic reasons. So I started from [a clean vim setup](https://github.com/atil/vimfiles/blob/6af2727e026fe5a6d31a54a60a710af10448460e/vimrc) and added a specific functionality when I actually needed it. At several points in time I wanted to switch to a Linux environment because terminal and being minimal with Arch and customization and all that sort of impulses. However I convinced myself not to do so with one question: "Do I need it?". As you can guess, the answer is no. Cargo works perfectly fine with Windows cmd (though I must admit, [Windows Terminal](https://github.com/microsoft/terminal) _does_ add value to the experience) and I don't use cmd for anything other than cargo and git. You see, I don't need even Cmder.

(Now this is _my_ line of thought, but I'll admit [Jon Blow's streams](https://www.youtube.com/playlist?list=PLmV5I2fxaiCI9IAdFmGChKbIbenqRMi6Z) and his way of working is an inspiration.)

A similar mindset is valid also for the choice of not going with any frameworks like [glium](https://github.com/glium/glium), [gfx](https://github.com/gfx-rs/gfx/) and [luminance](https://github.com/phaazon/luminance-rs). Having almost everything handmade gives the experience of knowing how things run under the hood and doing stuff in the exact way you want. (I exclude SDL though. SDL is fine. For now)

As for the answer to the question "why Rust?". I'm not gonna go and praise Rust here, you can find a whole heap of answers with a google search. Here, I'd answer "I don't exactly know" for now. But in the future, the answer is going to include something like 'being part of the change you want to see in the world'.

## Retrospective

That being said, the way Rust is designed makes me cover 100% of the cases, all the time. Which kind of contradicts with "having only what you need" mentality. I didn't find myself fighting with the borrow checker (yet), but I remember going out like "come ON Rust, I _know_ that it's not gonna be a problem!". However, Rust being the prim father figure, prevents you from going reckless and forces you to write the correct code. It's safe to assume that I've done less debugging than the case I'd have done it in C. I've come to think that comparing Rust against C is like comparing driving in Munich and driving in Istanbul.

But oh boy did I do me some grindy debugging… See, doing things the manual way makes you realize that the tools we use actually solve a metric crap-ton of problems for us.

The primary example being the character controller. I began with converting the [Unity FPS Controller](https://github.com/atil/fpscontroller) to Rust. The coding itself and making it move on a flat surface is easy, but the collision resolving isn't something you hack and slap in within a couple of days. The 3D math involved is a minefield of potential bugs if you write the thing yourself from ground up since it's easy to miss cases. Besides, it's difficult to track down which part of the calculation causes the error if you're checking the case _while_ playing the game, because things are flowing in real time and (without a debugger) you got to print a lot of information.

Thankfully though, Rust makes it very easy to write unit tests. For the first time in my life, I've followed a TDD-ish approach while actually getting work done and yeah it's amazing! I've caught errors just by adding test cases. Then, when I see a funkiness while running around in the test parkour, I got the erroneous triangle and the result and made a test case out of it. Now, going commando has again its perks here, primary being you know what's being run and there's no blackbox to throw assumptions at. I drew the case on paper and ballparked what the result should be, then made a unit test out of it for actual debugging. (By actual debugging I mean prints. I think the only humane debugger experience for Rust exists with CLion IDE at the time of writing)

Let's move onto a bit more technical anecdotes.

When it comes to debugging, I guess every graphics programmer out there will give a nod when I say 'OpenGL problems'. I'll tell you why:

I implemented flying movement controls, which are toggled via a key. I go into the fly mode (where the gravity doesn't apply and collision isn't checked) and then when I toggle it off, I continue walking around and jumping. Now, this feature is pure 3D math being done by my side, with Rust, on CPU.

The game crashed when I toggled off fly mode.

Now you know why.

This bug I've fixed literally today, at the time of writing, has turned out to be something related to some vertex buffers being bound to some other vertex arrays in UI which happen to be invalid from the previous frame in this particular case and so on, I still don't exactly know. But man, the journey between the two points was literally changing random OpenGL calls until I see something different on the screen. The thing crashes with a segfault, and your #1-in-stackoverflow-survey-friend Rust with it's fancy borrow checker isn't any help here. Because that's OpenGL. And if you're relatively new to it, God help you if even your glGetError() function ends up in a segfault. And the problem is, things go beyond the point at which you can google stuff and find the answers on why OpenGL behaves the way it does. The subtle problems which creep into the code path as you build up your architecture come bite you in the back, and that bite is unquestionably painful if you have no idea what you're doing (like me) or how OpenGL works under the hood.

Another workaround solution took place with shaders and how to compile them. In one of Jon Blow's streams I saw that you can write both vertex and fragment shaders to the same file and use preprocessor directives to separately compile them. I immediately got hooked because currently the shaders are very simple and opening and editing two files is more cumbersome if you have the option to have it one file. Googling quickly brings up [this page](https://software.intel.com/content/www/us/en/develop/blogs/using-ifdef-in-opengl-es-20-shaders.html) which looks legit enough and straightforward; just sending an extra string "#define Stuff" instead of one (shader source) to the compile function. So (I think) I managed to convert the code to Rust and tried it. And, I kid you not, the thing went so sideways (of course it crashed, that doesn't count as something unusual) that printing a random string before (or after) the compilation call made it work. Now, I won't go on and ramble about the mysterious working ways of OpenGL, but the real problem of mine is that _what went wrong isn't apparent at all and I'm trying to do something straightforward_. Hours went into debugging attempts and googling. Ended up with concatenating strings with format!() macro and sending in one string. It felt like […yeah](https://youtu.be/kQKrmDLvijo). In retrospect, it made a lot of sense doing it this way.

One final attempt was trying to replace SDL with [glutin](https://github.com/rust-windowing/glutin), with the purpose of having a more Rusty solution without an additional .dll file to make things run. Haven't gone very far with the attempt; gave up after realizing that I potentially need to restructure my code because the main event loop of glutin takes a closure and any reference I feed into the closure needed to have 'static lifetime. I stopped the moment I realized I was about to fight with the borrow checker and reverted back to give this another try in the future when I have a better understanding how lifetimes and closures work. After all, I don't need to have a Rusty solution _at this point in time_.

## Next

The following milestone is going to have a more atmospheric one which is gonna reflect the experience better, with preliminary lighting and SFX. I'm curious what it'll end up being; this is gonna be the first time the thing is going to feel like something. Let's see.
