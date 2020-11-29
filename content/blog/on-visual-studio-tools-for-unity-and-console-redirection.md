+++
title = "On Visual Studio Tools for Unity and Console Redirection"
date = 2016-01-12
+++

Using Visual Studio when working with Unity is awesome and [Visual Studio Tools for Unity](https://visualstudiogallery.msdn.microsoft.com/8d26236e-4a64-4d64-8486-7df95156aba9) takes the comfort even further. Since Unity's 5.2 upgrade, VSTU comes integrated with VS. Though with this integration, the developers took away some of the flexibility.

For instance, I would like my VS to have a very clean view. VSTU's relaying Unity's Debug.Log's to VS's console spoiles this neatness a considerable amount, since it opens and re-opens console window without my permission, as log prints keep coming. Before 5.2 integration, it used to have a nice configuration menu wihtin Unity, where one could toggle this feature. Now, appearently, there is [not](https://www.reddit.com/r/Unity3D/comments/3qkq67/how_to_switch_off_unity_errors_in_visual_studio/).

**(EDIT):** There [is](http://unityvs.com/documentation/changelog/) now with version 2.3. The code below is unneeded now.

When I asked, the developer was kind to give the [answer](https://twitter.com/sailro/status/684132820533444608), which is an editor script that toggles console redirection:

\[code language="csharp"\] /\* With the native integration we don’t have a menu to setup options anymore. That’s because the majority of VSTU options are now directly available within Unity project settings.

But this is not the case for the console redirection.

You should do something like this with an Editor script to properly setup what you want: (only once, this setting is global). \*/

using UnityEngine; using SyntaxTree.VisualStudio.Unity.Bridge.Configuration; using UnityEditor;

public class Configuration : MonoBehaviour { \[MenuItem("VSTU/Enable or Disable SendConsoleToVisualStudio")\] static void SwitchSendConsoleToVisualStudio() { var cfg = Configurations.Active; var status = (cfg.SendConsoleToVisualStudio = !cfg.SendConsoleToVisualStudio) ? "enabled" : "disabled"; EditorUtility.DisplayDialog("VSTU", "SendConsoleToVisualStudio is now " + status, "OK", ""); } } \[/code\]
