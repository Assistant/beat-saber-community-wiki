<!-- TITLE: FAQ -->
<!-- SUBTITLE: Frequently Asked Questions! -->
# FAQ
## How do I load custom songs?
Follow the instructions in the [Beginners Guide](beginners-guide).

## Where do I find custom songs?
[Beat Saver](https://beatsaver.com/)
[Beast Saber](https://bsaber.com/)

We also have a dedicated channel on the Modding Group Discord: [`#finished-maps`](https://discordapp.com/channels/441805394323439646/442342190060929055/)

## How do I load plugins that aren't in the Mod Manager?

After you have run the Mod Manager, you can then add other plugins / mods in the following directory:

`.dll` plugins go in the directory:
|  |  |
| --- | --- |
| Steam | `steamapps\common\Beat Saber\Plugins` |
| Oculus | `hyperbolic-magnetism-beat-saber\Plugins` |

Asset replacement plugins go in the directory:
|  |  |
| --- | --- |
| Steam | `steamapps\common\Beat Saber\Beat Saber_Data` |
| Oculus | `hyperbolic-magnetism-beat-saber\Beat Saber_Data` |

**Can't find your install directory?** See: [Beat Saber install folder](faq/install-folder)

## Multiplayer?
Here is a guide to setup the multiplayer plugin: https://bs.assistant.moe/Multiplayer/

## My game updated and now none of my mods are working!
Each time the game updates it is possible *(and very likely)* that your existing will stop working and need to be updated.
The devs realized this, so when the game updates and you run it for the first time, everything in the `Plugins` folder is moved into the `Incompatible Plugins` folder. **Leave those plugins in there!**

To get mods back, simply **run the mod manager again.**
The mod manager only includes mods that have been confirmed to work on the latest version of the game!

Get the mod manager from the [Beginners Guide](beginners-guide).

## Should I use wrist weights when I play?
> **DO NOT USE WRIST WEIGHTS!**
{.is-danger}

You will only hurt yourself. Besides, a lightsaber wouldn't be heavy, *it's made of plasma.*
Please read [this comment](https://www.reddit.com/r/Vive/comments/8g9jgs/beat_saber_has_now_released/dya1yl7/) for more information.

## My game will not load!
You may have a popup on your desktop asking about your Beat Saver auth token to vote on songs at the end of the game.
If you don't care to vote on songs, simply click **"Cancel"** and the popup will not appear again.

## I tried to install mods via the Mod Installer but it gives me an error that it can't find the IPA.exe.
Try installing the [Song Loader](https://github.com/xyonico/BeatSaberSongLoader/releases) on its own, then try running the Mod Manager again.

## I want to double the BPM of my song, is there an easy way to do that?
Here is a [Python3 Script](https://cdn.discordapp.com/attachments/442372806705938434/447910905972523008/beat-saber-time-multiplier.zip) that will multiply the BPM and `_time` values in your JSON file by any given factor.

## Why are the vote buttons greyed out in game?

To vote on songs, you must have an account on Beat Saver, then input your access token in the modprefs file.

`\Beat Saber\UserData\modprefs.ini`

Change the value for: `apiAccessToken=replace-this-with-your-api-token`

Use the value from your beatsaver account by going to the access tokens page on your profile.
https://gyazo.com/46da60331d8bfabd4833ab85bcb73535
