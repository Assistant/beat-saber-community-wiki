<!-- TITLE: Example Mod -->
<!-- SUBTITLE: Learn how to make a basic mod for Beat Saber! -->

> This page is subject to changes at any time. Feel free to look often for new, missing, or changed content.
{.is-warning}

> Before you start, please be sure you've read the [template setup guide](/modding/intro) to download the plugin template and set up your project.
{.is-warning}
# Plugin Beginnings

If you followed the [template setup guide](/modding/intro), you should end up with something similar to the following below:

![Plugin Example](/uploads/modding-example/plugin-example-2.png "Where We Left Off")

Before we continue on with the example, we will assume that you have:
* A basic to intermediate understanding of C#
* A basic to intermediate understanding of the ins and outs of Unity (GameObjects, Components, etc.)

## A Quick Summary
In this example, we will work on a basic plugin that counts how many notes we miss or hit incorrectly while playing a song. This example plugin will utilize:
* An empty GameObject which will hold a Component class file.
* TextMeshPro meshes.
* A basic overview of events and actions.

## Setting Variables
> If at any point you feel overwhelmed, it probably means you do not have enough experience. In that case, consider watching some Unity and C# tutorials to get familiar with Unity.
{.is-danger}

Before we begin with our plugin, it is important to create some variables that will help us later in development:

![Plugin Variables](/uploads/modding-example/plugin-variables.png "Plugin Variables")
*(Courtesy of [Beat Saber Progress Counter](https://github.com/Strackeror/BeatSaberProgressCounter/))*

| | |
| --- | --- |
|`env` | The `0.11.2` patch of Beat Saber brought with it a different way the game loads scenes. Instead of one "master" scene, it is split up into different scenes depending on the environment. We will detect if the game is played a game based off the name of the scene.|
|`enabled` | Toggles our plugin on or off with the flip of a variable. If you want to add a configuration file later on, this will be one option you don't want to miss.||
`counterPosition` | Where our counter will be located in the scene.|

> If you receive an error relating to the Vector3 variable, you may need to add `using UnityEngine;` to the top of your file.
{.is-danger}

## --verbose
Let's add a quick line of code to the beginning of our `OnApplicationStart()` void function:

```csharp
Console.WriteLine("Hello World!");
```

It is useful to modify the starting parameters of Beat Saber (through Steam or a shortcut) and add `--verbose` to the end. This will start Beat Saber with a console window open, displaying logs from every mod you have installed. If you compile then launch Beat Saber, and scroll from the top of the console window, you should see our Hello World message. Any errors that would cause our plugin to not work properly will be shown here, although because our plugin is in a compiled state, it will not tell us the exact line number. Be careful!
## Creating a GameObject
We will write little code inside of `Plugin.cs`. We will create a MonoBehavior script which will handle the heavy lifting for us. In fact, we will only be writing three more lines of code before we move on to another file.

![Plugin Scenechanged](/uploads/modding-example/plugin-scenechanged.png "Plugin Scenechanged")

> If you receive an error relating to `env.Contains()`, you may need to add `using System.Linq;` to the top of your file.
{.is-danger}

### What's going on here?
Instead of having an if statement inside an if statement, we've compressed the code into just 3 lines.

* The first line deals with the `enabled` variable we set earlier. If it is `false`, C# will take the inverse, which will be `true`. It'll activate the if statement, causing the function to return, and stop execution.

* The second line deals with the `env` string array we also created earlier. If the name of the scene is included in that array, it continues to our last line. If it does not, it'll activate the if statement, causing the function to return. This prevents our plugin from activating in the main menu, the tutorial, or in the very beggining "Health Warning and Information" screen.

### What's with the error?
Our plugin does not have a `MissedCounter` file to plug into our last line. Here we are creating an Empty GameObject at the very center of the scene, and adding a missing script into it. This is what we're going to be creating right now!

## MissedCounter.cs
Let's create a new `MissedCounter.cs` file that extends from `MonoBehavior`. For those who don't know, go to `Project` >> `Add Class...` and type in the name. In the class name, add ` : MonoBehavior` to the end.
> If you receive an error extending from `MonoBehaviour`, you will need to add `using UnityEngine;` to the top of your file.
{.is-danger}

![Plugin Missedcounter](/uploads/modding-example/plugin-missedcounter.png "Plugin Missedcounter")

For those who are familiar with Unity, you will recognize what we've just made. Here we can import Unity's default `Awake()`, `Start()`, and `Update()` functions, and you can pretty much go nuts from here.

# TextMeshes, Events, and Counting
In the previous section we covered the basics of any plugin. We created basic variables and have created an empty GameObject that will be called once a song is loaded. In this section, we will finish our component we added into the empty GameObject.

## References and TextMeshPro

If you [followed the plugin setup guide](/modding/intro), you learned how to add References when you set up the project. In this example, we will be adding `TextMeshPro` as a Reference to gain access to custom text in Beat Saber.

If you have forgot:
1. Under `Project`, click `Add Reference...`
2. Under `Browse`, click the `Browse` button. You should be where you left off when adding References. Search for and then add `TextMeshPro`.
3. In `MissedCounter.cs`, add `using TMPro;` to the top of the file.

![Plugin Addreferences](/uploads/modding-example/plugin-addreferences.png "Plugin Addreferences")

## Adding Variables

Before we can make a working counter, we need to make some variables that will help us in this task. Let's create these variables:

![Plugin Variables 2](/uploads/modding-example/plugin-variables-2.png "Plugin Variables 2")

### What are these variables?
| | |
| --- | --- |
| `counter` | A simple counter that will increment every time we hit a note incorrectly, or miss a note entirely. |
| `score` | Our reference to a Beat Saber component which has two events we will use, `noteWasCutEvent` and `noteWasMissedEvent`. |
| `counterGO` | A second GameObject we will create which will act as a label letting people know that it is counting misses. |
| `counterText` | The TextMeshPro mesh we will add to our component and update when we miss. |

## Resources

Before we continue in initializing, we will go over a key class that will aid a lot in Beat Saber modding.

`Resources` is a class that loops through everything inside a `Resources` folder in the Assets folder. This means we can search for a certain class and it will return it to us.

The most common use of this goes a little something like this:

```csharp
Resources.FindObjectsOfTypeAll<Saber>().FirstOrDefault();
```

This will loop through all objects and return the first (or default) instance of a `Saber` class popping up in `Resources`. This is mainly used for adding functions to events. For editing private fields and properties, you will have to use Reflection, which we will cover in a future time.

Resources can also be used to load in custom assets, fonts, images, etc.

## Grabbing our Score Controller

Before anything can work, we need to gain access to the `ScoreController` Beat Saber uses, and sometimes that will take time. There are many different ways to get access, but here are two commonly used and very similar methods.

In both cases, we will be the `Awake()` function built-in to Unity components which will start a function utilizing an infinite `while(true)` loop that will attempt to get the first `ScoreController`. If we find it, the loop will break and continue execution into an `Init()` function. If we don't, we'll wait for a bit before trying again.

### Coroutines and IEnumerators
> If you receive an error relating to `IEnumator`, add `using System.Collections;` to the top of your file.
{.is-danger}

This has been used since the beginning of Beat Saber modding, where the version of .NET was 3.6. With no `System.Threading.Tasks` and no asynchronous tasks, we had to rely on built-in MonoBehaviour scripts. We will be creating an `IEnumerator` which will hold our code, and starting it with the MonoBehaviour function `StartCoroutine`.

![Plugin Init Ienumerator](/uploads/modding-example/plugin-init-ienumerator.png "Plugin Init Ienumerator")

### System.Threading and Asynchronous Functions
> If you receive an error relating to `async` or `Task`, add `using System.Threading.Tasks;` to the top of your file.
> {.is-danger}

> If you receive an error relating to `Thread.Sleep()`, add `using System.Threading;` to the top of your file.
> {.is-danger}

As of the newest Beat Saber update (`0.11.2`), Beat Saber updated to Unity 2018, and with that comes the change from .NET 3.6 to .NET 4.x. This means we can use asychronous functions to handle tasks that will take time in a different thread and not cause the game to freeze! With newer Visual Studio releases, it should also automatically add `using System.Threading.Tasks;` to any newly created class file.

Here we will be turning our `Awake()` function into an async one and have it `await` a Task, which will hold our code.

![Plugin Init Threading](/uploads/modding-example/plugin-init-threading.png "Plugin Init Threading")

## Creating TextMeshes

Whichever method you chose in grabbing a score controller, you will end up in the `Init()` function. Here is where we will create our GameObjects, TextMeshes, and rearrange everything to fit in with elements in-game.

Let's begin with creating our two TextMeshes and GameObject. Remember, we will be adding a TextMesh to the existing GameObject, and creating a new one which will hold our label.

`counterText` will be added to the GameObject that also contains `MissedCounter`. We can access this by doing `this.gameObject`. `countGO` will be a new GameObject which will hold another TextMesh, which will act as a label. We did not create a second TextMeshPro variable in the [Adding Variables](/modding/example-mod#adding-variables) section because we do not need the label TextMesh once it is created.

Once we add the TextMeshPro Component onto our GameObject, we can edit every aspect of the text. Color, font, font size, alignment, and more, can all be edited through the variable we created. We can also apply the position of our `counterPosition` in `Plugin.cs` here, and add a bit of an offset to have our counter appear below the label.

We can pretty much copy and paste our code for our label TextMesh, although we will have to create a new GameObject for `countGO`.

![Plugin Creatingtextmeshes](/uploads/modding-example/plugin-creatingtextmeshes.png "Plugin Creatingtextmeshes")

## Adding Events

With the `score` variable we created then initialized earlier in the section, we can now access its properties. Two of these properties are events that we will add functions of our own into. These two events are `noteWasCutEvent` and `noteWasMissedEvent`. As you can expect, these will fire when a note is missed and a note is hit.

In the same `Init()` function, we will add an if statement seeing if our `score` variable is not null. If it is assigned (and not null), we will create two functions that handle these and add them to the respective events. Note that because these events have parameters that are added in, we'll add these as well.

![Plugin Adding Events](/uploads/modding-example/plugin-adding-events.png "Plugin Adding Events")

So we just create a function that increments the counter, right? Wait a minute! What if we miss a Bomb? Or if we hit the note correctly? Using `data`, which is a variable passed into our two handler functions, we can get information on the note that was hit and missed. And `info`, a variable found in our note cut function, helps us make sure that the cut was OK. Adding some if statements which filter missing bombs and making sure the cut was not OK, we can create our final function and have them call it.

![Plugin Conditionals](/uploads/modding-example/plugin-conditionals.png "Plugin Conditionals")

## Incrementing Our Counter

Our final function will be a simple one. Here we will implement our `counter` variable and set the text of the `counterText` variable we assigned in `Init()` to our updated value. This is easy since our variable was made outside of any functions, so we dont have to go digging and using `this.gameObject.GetComponent`.

![Plugin Increment](/uploads/modding-example/plugin-increment.png "Plugin Increment")

And with those final pieces of code added, we are finished with this example plugin! Now all we need to do now is to build it and move it to our plugins folder.

# Let's Build!
In the previous section, we have just finished our plugin and are ready to build it as a `.dll` and move it over to our Beat Saber installation. This section will focus on building, moving it over, and testing.

## Building Our Plugin

When your plugin is in a testable state (No errors, little warnings, everything's ready), go under `Build`, and click on `Build <Plugin Name>`. It is as easy as that! Now go to your `Output` window (Either go into `View` >> `Other Windows` >> `Output`, or it should be right next to your Error List), and it should tell you where your built file is located.

![Plugin Buildlocation](/uploads/modding-example/plugin-buildlocation.png "Plugin Buildlocation")

## Final Steps

Moving the file from building is just as easy as copying the build path into your File Explorer, opening another one into your Plugins folder of the Beat Saber installation, and drag and dropping. There really is no simpler explanation than that.

Then, launch Beat Saber, enter a song, and watch as a brand now Miss counter appears right below the Combo!

## Debugging
If you find your plugin not working, remember to launch Beat Saber in `--verbose` mode. This will open a console window where any errors will get logged and you can find out what went wrong. Another way to go about this is adding `try catch` statements in code where you think could be erroring, and logging something. This also prevents the plugin from breaking and no longer working at all after the first error.

# Before You Go
We hope you enjoyed this tutorial on making a custom plugin for Beat Saber. In this tutorial, you learned the basics of creating Beat Saber Plugins, and are now ready to go out and create your own.

Also, be sure to visit the [#mod-development](https://discordapp.com/channels/441805394323439646/443146108420620318/) channel on the [modding discord](https://discord.gg/beatsabermods), to share what you're working on! Feel free to also use it for help and suggestions based on your mod.

## Source Code
**If you find yourself stuck with an error in your code, and you want to compare with the project we've created, we've published the source code on GitHub: https://github.com/Caeden117/MissedCounter**
# Extras

Here is some extra content and information that will help you on your way of creating a plugin. Inspecting the game's uncompiled code, create and read settings, and access private variables are all things Beat Saber plugins do, so why leave you off with *just* the basics?

Here's programs and libraries that allow you to go more advanced with your plugins, and will help you greatly if you decide to explore new territory in Beat Saber modding. Remember, the [Beat Saber Modding Group](https://discord.gg/beatsabermods) is always here for your programming help!

## dnSpy
dnSpy is a .NET debugger and assembly editor that allows you to import compiled `.dll`s and view them decompiled into C#.
In the case of modding, we can use dnSpy to view the source code of Beat Saber and find methods and variables that would be handy to have for some plugins.
Read more about it [here](https://github.com/0xd4d/dnSpy).

Download the [latest release of dnSpy.zip](https://github.com/0xd4d/dnSpy/releases/latest).
Extract the zip, and run `dnSpy.exe`.
Then drop in the file `\<Beat Saber Directory>\Beat Saber_Data\Managed\Assembly-CSharp.dll`
`Assembly-CSharp.dll` contains nearly all of the games' code.

You can also use dnSpy to view and even edit the code of plugins, even those that have not yet published to GitHub. Be wary, though! Snooping around in other's code without permission is a bad idea. Also, modifying only a few lines of code can turn a harmless mod into a disaster.

![dnSpy Example](/uploads/modding/dnspy-example.png "dnSpy Example")

## Harmony
Harmony is a library for patching compiled .NET and Mono methods during runtime.
This allows editing of core Beat Saber functions and allows plugins to achieve things way beyond what basic Illusion Plugin Architecture can achieve.
Read more about it [here.](https://github.com/pardeike/Harmony)

Download the [latest release of Harmony](https://github.com/pardeike/Harmony/releases)
Extract `0Harmony.dll` to `/<Beat Saber Directory>\Beat Saber_Data\Managed`.
Add Harmony as a reference ([Click here if you forget how to add a reference to your project](/modding/example-mod#references-and-text-mesh-pro)).

> Plugins that utilize Harmony ***require*** the `0Harmony.dll` file inside of `/<Beat Saber Directory>\Beat Saber_Data\Managed`. Add a try-catch statement when you patch the game so the plugin doesn't crash when starting without Harmony.
{.is-warning}

## Reflection Util
ReflectionUtil is a simple file that allows grabbing and setting private variables inside of files.
This is used in conjuction with [dnSpy](/modding/example-mod#dnspy) to search for and set private variables in code.
The most basic version of ReflectionUtil can be grabbed [here.](https://gist.github.com/Caeden117/87c02dda25df90a0896292187f78f106)

Create a new file called `ReflectionUtil.cs`
Copy and paste the code to the newly created file, making sure to replace `namespace Your_Project_Here` with the name of your project.

In other files, objects will now include a `GetPrivateField<T>(name)` and a `SetPrivateField(name)` function. You can pass in the name of any private variable (Usually marked with the `private` protection keyword, and/or an underscore `_` before the name) into either, and either:
* A compatible file type for `T` (Use `Saber` when trying to get a Saber set as a private field) when using `GetPrivateField<T>()`
Or...
* A compatible object to set `value` to when using `SetPrivateField(name, value)`.

You can also manually pass objects in by using `ReflectionUtil.GetPrivateField<T>(obj, name)` or `ReflectionUtil.SetPrivateField(obj, name, value)`.

### Example
Let's say you have a `PlayerController` named `pc` and you wanted to get the left Saber. After setting up ReflectionUtil, all you need to do is:

```js
Saber leftSaver = pc.GetPrivateField<Saber>("_leftSaber");
```

> If the name of the private variable does not exist, the plugin will return an exception saying `Object reference not set to the instance of an object`.
{.is-warning}

## ModPrefs
Illusion Plugin Architecture plugins automatically include a form of saving and loading user content, called `ModPrefs`.

ModPrefs are saved in `/<Beat Saber Directory>/UserData/modprefs.ini`. Setting and getting preferences are now as easy as:
```js
//Sets the string in section "section" with name "name" with the value "value".
//You can also use SetInt, SetFloat, SetBool, etc.
ModPrefs.SetString(section, name, value);

//Gets the string in section "section" with name "name".
//Overwrites the value with "defaultValue" is "autosave" is set to "true".
//You can also use GetInt, GetFloat, GetBool, etc.
ModPrefs.GetString(section, name, defaultValue, autosave);
```

## Advanced Building
### Symlinks
If you find yourself tired of having to copy your finished builds from one folder to the next, this will help you minimize the hassle needed by only just needing to Build, then you are good to go.

A `symlink` is something that tells Windows that one file is actually linking to another directory. This is useful if you want a folder on your Desktop that goes straight to your Beat Saber directory when you open it, or in our case, have a symlink from our plugin in our Plugins folder to the latest builds in our build directory.

>If any directory has spaces (Example: `cd C:\Users\Test\Hello World`), encase the path in quotations: `cd "C:\Users\Test\Hello World"`

1. Open Command Prompt as Administrator.
2. Make sure you do not have your plugin inside your Plugins folder.
3. Have Command Prompt point to your Beat Saber Plugins folder (`cd <Beat Saber Directory>/Plugins`)
4. Execute the following command: `mklink <Plugin Name>.dll, <Path to your build file>`

If all is done correctly, the next time you build your plugin and launch Beat Saber, the plugins should update automatically.

### Post-Build Events
Another way to go about this is taking advantage of C#'s post-build events. This not only allows you to copy files to a directory when build is complete, but you can also do a lot more.

1. Under `Project`, click `<Project Name> Properties...`
2. Click `Build Events`
3. Copy and paste the following code into the Post-build event command line: `copy /Y "$(TargetPath)" "<Path to Beat Saber Plugins folder (Including the file name and extension)>"`
4. Save and exit.

If all is done correctly, the next time you build your plugin, a copy will be automatically sent to your Plugins folder (Automatically overwriting any previous version), and Beat Saber is ready to play.