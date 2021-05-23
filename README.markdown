Guide to Troubleshooting Victoria II
====================================

Despite its maturity Victoria II can be surprisingly hard to run nowadays. This guide, while not
exhaustive, aims to help you solve your issues.

Note that running Victoria II with mods can also bring its own share of issues. This guide will
occasionally mention mods when it makes sense to, but e.g. how to install and run mods is outside
the scope of this document.

Note on Steam betas
-------------------

This is not expected to solve anything but this is a convenient opportunity to dispel
misunderstandings. **Steam users do *not* need to be using the 3.04 beta.** Version 3.04 has been
the default [since 2016][3.04]. Steam users can safely opt out of all beta programs.

![Steam beta programs](./beta-programs.png)

[3.04]: https://forum.paradoxplaza.com/forum/index.php?threads/3-04-latest-official-patch.991138/

Understanding your Victoria II install
--------------------------------------
[user files]:          #understanding-your-victoria-ii-install
[user file structure]: #understanding-your-victoria-ii-install

A typical installation leads to the following file structures:

- <dfn>*Game files*</dfn> proper, necessary for Victoria II to run.

  <aside>

  For Steam users on Windows (depending on specifics and Steam library folder configuration) this
  typically looks like:

  ```
  C:\Program Files (x86)\Steam\steamapps\common\Victoria 2
  ```

  </aside>

- <dfn>*User files*</dfn>, for things like user settings & save files. This also contains
  mod-specific user settings & save files, e.g. [HPM] saves would be stored under `HPM/save games`.

  [HPM]: https://github.com/arkhometha/Historical-Project-Mod

  <aside>

  For Window users this is usually `%userprofile%\Documents\Paradox Interactive\Victoria II` (this
  should be a valid path regardless of localization, provided that user files are present).

  </aside>

Note that the game can work without user files and a fresh install starts without them. You might
not have a `Paradox Interactive` folder at all. The guide covers this situation below.

Using this guide
----------------

This guide divides known Victoria II issues into sections. Starting from the first, they each list
symptoms together with possible fixes. You should check whether some of these symptoms match your
situation. You shouldn’t expect absolutely all symptoms to apply, one or a couple should be enough.

If symptoms don’t match, skip ahead to the next section. If they do, see if the fixes help.

<a name="toc"></a>

| Table of Contents
|:-
| [The game won’t start](#the-game-wont-start)
| [The game starts, but I have no user files](#the-game-starts-but-i-have-no-user-files)
| [The game displays incorrectly](#the-game-displays-incorrectly)
| [The game displays, but is not borderless](#the-game-displays-but-is-not-borderless)
| [The game crashes when starting or loading a game](#the-game-crashes-when-starting-or-loading-a-game)
| [The game crashes when initialising the map](#the-game-crashes-when-initialising-the-map)
| [The game crashes when processing flags](#the-game-crashes-when-processing-flags)
| [The game displays text incorrectly](#the-game-displays-text-incorrectly)

The game won’t start
--------------------

<sup>[Back to the table of contents](#toc)</sup>

**Steam users, pay close attention to this section!** Important information has been
**highlighted**.

Symptoms:

- **this is a new install on a fresh system**
- **the unmodded game works fine, but the launcher can’t find mods or mods don’t work**
- **there is some error being displayed, such as a runtime error for C++** <!-- stay compact -->  
  ![MSVCP100.dll is missing](./msvcp100.png)
- **an error appears, but only when starting the `victoria2.exe` or `v2game.exe` executable directly
  in the game files and not through Steam**
- there is no error being displayed
- the game or launcher doesn’t even appear
- the game process barely lasts if at all, there’s just no game
- the [user files][] are missing, and the file structure is not being created by the game

This is typical of the Steam version, as it appears the installation process seems to (sometimes?)
neglect installation of required redistributables. **Steam users are highly encouraged to try the
following fix when encountering a problem, even when some or all of the rest of the game appears to
work.**

Fix:

- Install required redistributables (below are links to official Microsoft downloads):

  * [DirectX End-User Runtime][directx-redist]
  * [Visual C++ 2010 Redistributable Package (x86)][visual-redist] (make sure not to mistake this
    for the x64 redistributable as Victoria II is not a 64-bit game–the link points to the
    appropriate version)

    ***NOTE:*** as of this writing, the official download appears to be out of order. You can try to
    search for the package to find third-party distributors yourself, or [find it on
    cnet.com][third-party-visual-c++-2010-redistributable].

    [third-party-visual-c++-2010-redistributable]: https://download.cnet.com/Microsoft-Visual-C-2010-Redistributable-Package-x86/3000-2383_4-75451146.html

  Either of the installers may report that your system is up-to-date, which can look like the
  following:

  ![A newer version has been detected](./uptodate.png)

  This is a sign that things are in order for that particular redistributable, and no further action
  is required on your part. Move on to the next installer, if not done already.

  However if one of the installers otherwise reports an error during the installation process, this
  can be a sign of conflict or corruption. How to repair your system in such case is outside the
  scope of this guide.

[directx-redist]: https://www.microsoft.com/en-US/download/details.aspx?id=35
[visual-redist]:  https://www.microsoft.com/en-US/download/details.aspx?id=5555

Other remarks:

- Victoria II also requires the .NET Framework 2.0, which these days is included in the .NET
  Framework 3.5. If you are a Windows 10 user, you should have been prompted to enable it the first
  time you launched a .NET 3.5 or earlier application. You should not have to do anything else.

  If this does not cover your situation, you should [try the official documentation][.NET 3.5].

  (**Partially confirmed**) Since however the .NET Framework is only used by the launcher and not
  the game itself, you can work around the issue if you cannot manage to install this prerequisite.
  You can start the game directly by launching the `v2game.exe` executable itself. You can find it
  in you installation folder right besides the `victoria2.exe` launcher executable.

  If you want to load mods without the launcher, you’ll need to be adding `-mod=<mod file>` launch
  options, e.g. to a shortcut to `v2game.exe` (for Windows users). Multiple options can be used to
  load multiple mods. Detailed information on the topic is outside the scope of guide.

[.NET 3.5]: https://docs.microsoft.com/en-us/dotnet/framework/install/dotnet-35-windows-10

The game starts, but I have no user files
-----------------------------------------
[no-user-files]: #the-game-starts-but-i-have-no-user-files

<sup>[Back to the table of contents](#toc)</sup>

Symptoms:

- the launcher works
- the game starts
- there is no [user file structure][]
- there is a user file structure, but the `logs` folder inside is always empty when starting the
  game without mods
  * a `logs` folder full of empty individual `.txt` log files is not abnormal in
    itself however

<aside>

For Window users, this typically means there is either no `%userprofile%\Documents\Paradox
Interactive\Victoria II` folder (if you have played other Paradox games before), or not even a
`Paradox Interactive` one (if you haven’t).

</aside>

Possible causes & some fixes:

- **(Partially confirmed)** The device or filesystem where the user files are expected to live may
  not have any available storage left. When this is the case, the game is sometimes known to crash
  rather than tell the user what is happening. You will have to free up some space.
- You may have unusual ownership/permissions set for either your game files or your user file parent
  folders. (This can be the case if you are only able to start e.g. Steam or your games in admin
  mode.) This should only happen if you went out of your way to set it up so, and power users should
  figure out on their own how to fix this.
- There are ways to use a custom location for your user files. Once again this is more of a power
  user situation that shouldn’t happen on its own, and that will not be covered here.

From this point on, subsequent sections will assume that the user file structure is set up and
functional. In particular, the `logs` folder inside the user files is also where the game stores
logging information. (Starting the game with a mod enabled may result in information being stored in
`<mod-path>/logs` instead.) This can be useful for figuring out what goes wrong with the game.

The game displays incorrectly
-----------------------------

<sup>[Back to the table of contents](#toc)</sup>

Symptom:

- the game runs (there is a game process) but it’s being displayed incorrectly:
  * nothing is being displayed
  * the screen is black
  * the resolution is wrong
  * there is a lot of flickering (the refresh rate is wrong)
  * there is no mouse cursor
  * it’s hard to click on the right spot of the user interface

While the game lets you set video options, this is not of much help if the display is so wrong the
user isn’t able to click anything!

Fixes:

- In your [user files][] there should be a `settings.txt` file. You can edit it directly when the
  game is **not** running to change how the game will display on the next start.

  Here is for instance what the `graphics` entry of the file can look like for running at 1080p
  60 Hz in borderless mode:

  ```
  graphics=
  {
  size=
  {
          x=1920
          y=1080
  }

  refreshRate=60
  fullScreen=no
  borderless=yes
  shadows=no
  shadowSize=2048
  multi_sampling=8
  anisotropic_filtering=0
  gamma=50.000000
  }
  ```

  Besides of course using a resolution appropriate for your screen, here are some things that may be
  worth investigating:

  * Using full screen:

    ```
    fullScreen=yes
    borderless=no
    ```

  * Paying close attention to the refresh rate. Some screens can be running at 59.94 Hz (sometimes
    displayed as 59 Hz) or 61 Hz and you should verify what your system and/or other games are using.
    Victoria II is known to sometimes guess an incorrect refresh rate, making it unusable.

- You may not have a `settings.txt` file, worry that it is broken, or not be confident that you can
  correctly edit it by yourself. For these cases we provide a model file that you can [review to use
  as a template][settings-model-file] or [download wholesale][settings-model-download].

  When doing the latter, place the file at the root of your user files **as long as you understand**
  this will affect *all* your settings. (As is tradition for Paradox games you might want to put
  your headphones down for the first start!) Typical window users would end up with the following
  file:

  ```
  %userprofile%\Documents\Paradox Interactive\Victoria II\settings.txt
  ```

  [settings-model-file]:     ./settings.txt
  [settings-model-download]: https://github.com/moretrim/victoria2-troubleshooting/releases/download/v1.0/settings.txt

Note that the above applies to the unmodded game. Some mods rely on a mod-specific user file
location. If you are encountering this issue while starting the game with such a mod enabled, you
can try the same steps at that location. For instance for a typical [HPM] installation on Windows
you would be tweaking the following file instead:

```
%userprofile%\Documents\Paradox Interactive\Victoria II\HPM\settings.txt
```

To make your life easier you can take care of a single master `settings.txt` file as long as you
copy it over to each of your mod user file location whenever you change it. (As an added bonus you
can likewise copy your `messagetypes_custom.txt` files around to transfer your message settings
between mods.)

The game displays, but is not borderless
----------------------------------------

<sup>[Back to the table of contents](#toc)</sup>

Symptom:

- the game runs & displays, but:
  * setting `borderless=yes` in the `settings.txt` file has no effect, and the entry is being
    removed from the file by the game
  * in the settings screen of the game, there is no option for setting borderless

Cause:

The option to run the game borderless was added very late in the game’s development. As a
consequence, it is only available to users with **the final <cite>Heart of Darkness</cite>
expansion**. (In this era of Paradox Development Studio games, on-going development only applied to
the latest expansion meaning that patches & fixes were not backported to earlier expansions or the
base version of the game.)

This is what part of the video settings screen should look like when you are running <cite>Heart of
Darkness</cite>, with the window mode option highlighted:

![Heart of Darkness video settings screen](./borderless-setting.png)

Note the expansion title & background image.

Fixes:

There are tools to make a windowed application appear borderless. One such tool that is known to
work for Victoria II is [Borderless Gaming][]. (Make sure you are running the game in windowed mode:
either set it through video settings, or [edit your `settings.txt`](#the-game-displays-incorrectly)
to contain `fullScreen=no`. Remember that mods may rely on their own `settings.txt`!) How to use
these tools is beyond the scope of this guide.

While using these tools, you may have difficulty scrolling the map with your mouse cursor as it
escapes the borderless window. You can use the arrow keys to mitigate this, or investigate whether
the tool supports a mouse locking feature. Borderless Gaming has one such feature under its options.

[Borderless Gaming]: https://github.com/Codeusa/Borderless-Gaming/releases

The game crashes when starting or loading a game
------------------------------------------------

<sup>[Back to the table of contents](#toc)</sup>

Note that save games are part of the [user files][]. Ensure [these are working][no-user-files]
before reading this section.

Symptoms:

- the game is stuck or crashes when attempting to display the campaign screen before starting or
  loading a game, after choosing “Single Player” in the main menu

Fixes:

- **(Partially confirmed)** The game can crash when it fails to process some saves. It is suspected
  that this can happen for corrupted saves, or saves that were made with a different game or mod
  version. On a typical installation on Windows saves are located at the following:

  ```
  %userprofile%\Documents\Paradox Interactive\Victoria II\save games
  ```

  (If you are loading a mod you may be using mod-specific user files instead, in which case the save
  games are found in a `<mod-path>/save games` folder inside your user files.)

  You can stow away your old saves in a subfolder to hide them from the game. E.g.:

  ```
  %userprofile%\Documents\Paradox Interactive\Victoria II\save games\my old saves
  ```

  Other solutions may involve hiding those saves inside an archive file, moving them somewhere else
  altogether, or outright deleting them.

The game crashes when initialising the map
------------------------------------------

<sup>[Back to the table of contents](#toc)</sup>

Note that the logs and the map cache are part of the [user files][]. Ensure [these are
working][no-user-files] before reading this section.

Symptoms:

- the game is stuck or crashes on the initial loading screen when initialising the map logic or when
  the map is properly initialized
- the game is stuck or crashes on or soon after startup, and the last thing that the logs mention is
  related to the map

Fixes:

- **(Unconfirmed)** If the device or filesystem where the user files live has no available storage
  left, you will have to free up some space.

- Initialising the map logic for the first time is an expensive operation. If the game is not
  crashing, make sure you are giving the operation a chance by leaving it some time to complete.

- You can clear the map cache, either by deleting it or by emptying it of all files. The map cache
  is the `map/cache` folder in your user files. For Windows user, that is typically:

  ```
  %userprofile%\Documents\Paradox Interactive\Victoria II\map\cache
  ```

  If you are loading a mod you may be using mod-specific user files instead, in which case the map
  cache is a `<mod-path>/map/cache` folder inside your user files.

- Steam users may try verifying the integrity of game files, in case the map files were corrupted.
  (This does not compromise your settings or save files.) If Steam find files to correct, consider
  clearing the map cache before starting the game.

- Other users may try reinstalling the game & clearing the map cache.

The game crashes when processing flags
--------------------------------------

<sup>[Back to the table of contents](#toc)</sup>

Note that the logs and the flag cache are part of the [user files][]. Ensure [these are
working][no-user-files] before reading this section.

Symptoms:

- the game is stuck or crashes on the initial loading screen when processing or loading flags
- the game is stuck or crashes on or soon after startup, and the last thing that the logs mention is
  related to flags

Fixes:

- If the device or filesystem where the user files live has no available storage left, you will have
  to free up some space.

- Initialising the flag cache for the first time is an expensive operation. If the game is not
  crashing, make sure you are giving the operation a chance by leaving it some time to complete.

- You can clear the flag cache, either by deleting it or by emptying it of all files. The flag cache
  is the `gfx/flags` folder in your user files. For Windows user, that is typically:

  ```
  %userprofile%\Documents\Paradox Interactive\Victoria II\gfx\flags
  ```

  If you are loading a mod you may be using mod-specific user files instead, in which case the flag
  cache is a `<mod-path>/gfx/flags` folder inside your user files.

- Steam users may try verifying the integrity of game files, in case the flag files were corrupted.
  (This does not compromise your settings or save files.) If Steam find files to correct, consider
  clearing the flag cache before starting the game.

- Other users may try reinstalling the game & clearing the flag cache.

The game displays text incorrectly
----------------------------------

<sup>[Back to the table of contents](#toc)</sup>

Symptoms while in the game:

- the text is slightly offset
- the text has incorrect spacing
- the text is all white/black boxes instead
- the text is incorrect (e.g. the ‘Exit’ button displays something nonsensical instead)

This can be caused by mangled localisation files.

Fixes:

- if this happens to the unmodded game, verify the integrity of game files (for the Steam version)
  or reinstall the game—make sure that game files (but **not** user files, normally: you should be
  able to keep your saves!) are properly being cleared after uninstallation
- if this happens to the game with a mod enabled, verify that you correctly downloaded the mod’s
  files and placed it at the right location (how to do this will not be covered in this guide)
