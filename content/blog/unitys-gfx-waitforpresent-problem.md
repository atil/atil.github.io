+++
title = "On Unity's Gfx.WaitForPresent Problem"
date = 2015-11-10
+++

In many Unity projects I've worked on, I've encountered those green spikes seen in the profiler, under the name Gfx.WaitForPresent, which chops framerate down to hell. This basically means that CPU's waiting time for GPU to finish its job, in other words, that spike you see is not the problem itself. I guess it probably indicates that some rendering.. thing.. is horribly conflicting with another, and my googling failed to give me a definite answer of reason why. I'll mention some options to try, mostly compiled from [here](http://forum.unity3d.com/threads/gfx-waitforpresent.211166/), for not having to look at the same inconclusive forum threads again and again. Some of them doesn't make any sense initially, or may be fixed in time, but lack of definite solutions made me keeping them here anyway:

- Most people mention disabling V-Sync first. So if you are reading this, you've probably tried that and it didn't work.
- I've seen some post processing effects really don't get along with specific hardware. Start looking at things from there.
- Disabling shadows and anti-aliasing from quality settings apparently helped some people.
- Running Unity with "-force-opengl" argument.
- Deactivating gizmos.
- Running Unity on primary display.
- Running Unity with XP compatibility mode.
- [Some guy](http://forum.unity3d.com/threads/gfx-waitforpresent.211166/page-3#post-2021409) claimed that zooming in on Photoshop temporarily reduced the spikes. (I myself tried moving profiler window around, and it worked)
- [Another one](http://forum.unity3d.com/threads/gfx-waitforpresent.211166/page-4#post-2095011) mentioned getting texture MaxSize's right.
- Closing Chrome sounds desperate, [but...](http://forum.unity3d.com/threads/gfx-waitforpresent.211166/page-4#post-2104225)
- Your game (or Unity editor) may be running on integrated graphics card, if you are working on a laptop. Try switching it to GPU.
- Though may not cause spikes, too many seperate transparent objects may trying to be drawn. Check transparent objects
- (EDIT) The problem may be related to [multithreaded rendering](http://forum-old.unity3d.com/threads/gfx-waitforpresent-yes-another-one.382370/) and [turning it off](http://www.torestudios.com/news/unity-android-gfx-waitforpresent-issues/) may work
- (EDIT) F.Lux (a software to make monitor colors more eye-friendly) seems to be messing things up as pointed out [here](https://www.reddit.com/r/Unity3D/comments/4tdoa4/psa_if_your_are_you_are_having_problems_with/)
- If all above fails, try removing everything except Assets folder.

Note that this post is about "spikes", which indicates not necessarily a GPU bound situation. If your profiler have somewhat steady amount of Gfx.WaitForPresent, your game may be suffering from some heavy rendering tasks.

If you too have had the same issue and solved it, I'd love to hear from you.
