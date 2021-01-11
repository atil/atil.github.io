+++
title = "Fabric 2 Dec '18 - Jan '19 Devlog: The Foundation and the Direction"
date = 2019-02-03
+++

So, December was spent with vacations mostly. First, a week back at home and then Xmas kept me a little distant from the game. So I thought maybe I can wrap two months up together with one post.

November's drudge work of getting the editor up and running was finally finished by the beginning of December. Level creation - editing - testing cycle has become relatively painless. When wearing the level designer hat, I'll only have a couple of shortcuts to interact with the whole thing. Unless the underlying tech changes completely, I won't be adding new big features to editor. At least until the next big milestone, which I'm gonna talk about in a second.

Then I moved on to the visuals. So, we're going with a programmer-style-minimalistic look, which is just one flat color per brush. It might look cool and all (and I honestly don't think it does), but there's a problem about the character movement: It's really hard for the player to get a feeling of how fast the character is moving, or how high the character jumps, due to the lack of movement on the screen since there's just one color for the whole floor. This is especially prominent when there's just a flat surface and nothing else. To address this problem, I had added some non-uniformity to the brush colors using simplex noise. There were darker shades on the brush at seemingly random places. I also had stole the grid effect from the first game, which gives the player a somewhat clear feedback about jumping and landing. But that's all scrapped out, due to the big guy that barged into the house. Enter tessellation.

From the beginning, I knew that bending has boring look. It was just meshes scaling down in one axis. When compared to the wiggle effect of the first game, it definitely looked bland and felt empty. Since bending operation is the main tool the player has, it is a major focus point when it comes to polishing. It has to feel absolutely "… whoa". Every line of thought I had about achieving this ended up with some vertex effects, a reminicent of the first game. However, since the meshes are basically a bunch of scaled cubes batched together, I didn't have vertices to wiggle all over the place; I had to generate them somehow. One way came to mind was to insert vertices to meshes at compile time. But getting vertex positions and indices correct and building the correct triangles felt difficult and tedious to verify. It also meant that there would be potentially hundreds of thousands of new vertices to be sent from cpu to the gpu, since I don't know how big the levels might get in the end. Another way was the thing called tessellation. But I only had a vague idea about it being relevant to producing triangles within the pipeline. Fortunately there's a very explanatory tutorial in [Catlike Coding](https://catlikecoding.com/unity/tutorials/advanced-rendering/tessellation/). Though the problem was, I wasn't able to understand what's going on even after the second read. So I decided to copy-paste the whole thing and see if I can get the very basics running in the brush shader. I managed to locate where I can inject my triangle-wiggling noise and the line colors themselves, then I connected it to the bend logic. Here's the result:

![](bend3.gif)

I must say that it wasn't the most informative experience for me. There still is a bit of code that I've no idea what function they're fulfilling, like domain/hull shaders, and how lighting works with the generated triangles. After one point, I stopped stripping out useless code and decided not to touch it, except for the bend wiggle FX parts.

There was a major downside of using a tessellation shader: it effectively broke edge detection, which was another shader magic world that I've no idea about. I could see it working with bend tiles, which are plain meshes, but it effectively ignored the brushes. So I did what any clueless game developer would do, I began to try every single free edge detection asset out there. If one wasn't working out of the box, I tried to play with the sliders. If that wasn't working either, I was ditching the asset. Then I came across [this one](https://github.com/gregoiredelzongle/Gre-Sketch) which seemed to at least acknowledge the meshes with tessellation shader are there. How, I still have no idea. That was a start, but I saw that it completely ignored some edges. I wanted to fix that, and taking courage from the asset being a seemingly simple one, I jumped into the source code. I could manage to figure out the very basics. After a couple of weeks and blog posts, one of which was the [tigsource thread](https://forums.tigsource.com/index.php?topic=37314.msg1010460#msg1010460) of Manifold Garden, I sort of figured out how edge detection works in a fundamental level. It was around that time in the company I'm working at the guys were organizing an internal knowledge sharing mini-conference, so I thought maybe I can talk about this. And I did, so apart from Fabric I have [this](https://github.com/atil/edgeDetection), to use it in the future games. Funnily enough, my own solution doesn't work with the tessellation shader. Currently, my only way is to stick with what seems to work, without having a hope in understanding how it does.

While this edge detection work spanned along probably the whole January, meanwhile I was working on getting the features up and running. I started implementation with the triggers, since I knew I certainly was going to need them. They are just invisible volumes in three dimensional space, calling function whenever the player penetrates them. The initial use case was with toggling the unbend ability. With a fair certainity, I won't want the player to unbend before he/she sees the 'right click to unbend' on the wall. In the future, these triggers will come in handy with storytelling elements, like narrator dialogues.

The next was the button. It's basically the same thing with Fabric: pressing button unlocks the exit, so the player has to locate and go to the button first. It essentially is the same thing with the exit; another point for player to reach. The real purpose of this feature, is to force player to be familiar with the environment by traversing it, to make him/her try to give a meaning to the space he/she is in, and construct a mental map of the level in the process. Realization had come rather late, that this aspect is going to support the main experience very well, since it also is about doing operations on a piece of mental space which the player constructs by himself/herself. Once I had come into this understading I asked myself, is there anything bad with having more than one buttons for a level? I couldn't find a negative answer to that, therefore we can have up to three buttons in a level now.

The last bit of gameplay code I've done this month was changing gravity. In truth, the code was pretty much there even before 7DFPS, but felt horrible. The gravity was abruptly changing with one button. So the main task to make it more perceivable by the player by making the rotations as little and smooth as possible. This required quite a bit of 3d math, and still is not perfect, but at least the player can probably understand what's going on without suffering a vertigo.

![](gravity1.gif)

There's one aspect that's not shown in the footage though. Since the gravity direction comes from the surface normal being shot at, it's not limited to one of the cardinal directions. If the player uses a slanted surface to sample the gravity direction, he/she'll be able to experience the world very differently; since, especially for this game, the player will get used to perceive floors and ceilings to be perfectly flat surfaces. One more way to pull the carpet under the player's feet.

So that has been roughly the last two months. While going through all of these, I have come up with a mid-term plan, a vertical-slice milestone. Needless to say, having milestones are always a must, and with this milestone I decided not to go in a path which leads to a final product, but rather a half-way product to re-evaluate things when I reach there. This is mainly because there isn't a solid design, or a product scope yet. Even the main experience is not set in stone. On the other hand, I have things in my mind which I want to try out to see if they work. I plan to bring those things together with the players as soon as possible, and see what aspects exactly  
add to the experience and impress the players, and which don't.

So the vertical slice is going to feature all of the game's mechanics. There will probably be a hub world, and mini-campaigns that's gonna focus on a single mechanic. After completing each of these branches, the hub is going to unlock the next branch. At the very end there'll be a boss-like level in which the player will have to be a little bit fast in addition to the mental challenges of mutating space. There will be a few collectibles, with the purpose of gauging how engaging a secondary loop would be, a loop that's going to rely on a totally different mastery than the core puzzles provide: strafe jumping. Back in the day, I had built [a first person controller](https://github.com/atil/fpscontroller) to learn how physics of strafe jumping work, and used it in a couple of game jams, but now the time has come to test it in a real battle.

For all of these to work, I plan to be done with all the features first. I'm not going to go with the way of building one feature and exploring it as much as possible, like I've discussed [here](http://atilkockar.com/280-2/), because it basically has no end to it. I can do it for weeks and months. Besides, I'm not trying to come up with a final product at the moment, rather a vertical slice. Therefore the puzzle quality doesn't have to be top notch. Yet.

Another branch to work on is going to be the visuals. You probably can figure out how art-impaired I am from the fact that the colors I use coming from a code editor theme. The thing to do here will be to try as many [stolen color palettes](https://www.slant.co/topics/358/~best-color-themes-for-text-editors) as possible. Probably a couple of them will look better than the others. On the other hand, it's good to have something art related to work on, among all these code work, it'll feel refreshing when I'm exhausted from writing gameplay code and level design.

Lastly, I'm working on the story side of things as well. There are a couple of drafts now, though I'll have to decide in which form the game is going to present them in the vertical slice. Probably this will be the most fun part to work on.

That's it I for now guess. There'll be another blog post about not the progress of the project, but the purpose and the motivation behind it. Though I can't say when I'll solidify it in my mind.