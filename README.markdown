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
not have a `Paradox Interactive` folder at all.

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
- the user files are missing, and the file structure is not being created by the game

This is typical of the Steam version, as it appears the installation process seems to (sometimes?)
neglect installation of required redistributables. **Steam users are highly encouraged to try the
following fix when encountering a problem, even when some or all of the rest of the game appears to
work.**

Fix:

- install required redistributables (below are links to official Microsoft downloads):

  * [DirectX End-User Runtime][directx-redist]
  * [Visual C++ 2010 Redistributable Package (x86)][visual-redist] (make sure not to mistake this
    for the x64 redistributable as Victoria II is not a 64-bit game–the link points to the
    appropriate version)

[directx-redist]: https://www.microsoft.com/en-US/download/details.aspx?id=35
[visual-redist]:  https://www.microsoft.com/en-US/download/details.aspx?id=5555

The game starts, but I have no user files
-----------------------------------------

[user files]: #the-game-starts-but-i-have-no-user-files

<sup>[Back to the table of contents](#toc)</sup>

Symptoms:

- the launcher works
- the game starts
- there is no user file structure

<aside>

For Window users, this typically means there is either no `%userprofile%\Documents\Paradox
Interactive\Victoria II` folder (if you have played other Paradox games before), or not even a
`Paradox Interactive` one (if you haven’t).

</aside>

Possible causes:

- You may have unusual ownership/permissions set for either your game files or your user file parent
  folders. (This can be the case if you are only able to start e.g. Steam or your games in admin
  mode.) This should only happen if you went out of your way to set it up so, and power users should
  figure out on their own how to fix this.
- There are ways to use a custom location for your user files. Once again this is more of a power
  user situation that shouldn’t happen on its own.

Strictly speaking this isn’t an issue as the game can technically work without user files. However
you do need a place to put e.g. your settings and saves, so the topic has to be mentioned even when
some of it is outside the scope of this guide.

The real purpose of this section is twofold. First, some sections need to talk about user files and
will thus refer to this section.

Second, the `logs` folder inside the user file structure is also where the game stores logging
information. (Starting the game with a mod enabled may result in information being stored in
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

- In your user files there should be a `settings.txt` file. You can edit it directly when the game
  is **not** running to change how the game will display on the next start.

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

- If you have no `settings.txt` file or are worried it is broken, we provide a model file that you
  can [review to use as a template][settings-model-file] or [download
  wholesale][settings-model-download].

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

The game crashes when initialising the map
------------------------------------------

<sup>[Back to the table of contents](#toc)</sup>

Note that the logs and the map cache are part of the [user files][].

Symptoms:

- the game is stuck or crashes on the initial loading screen when initialising the map logic or when
  the map is properly initialized
- the game is stuck or crashes on or soon after startup, and the last thing that the logs mention is
  related to the map

Fixes:

- Initialising the map logic for the first time is an expensive operation. If the game is not
  crashing, make sure you are giving the operation a chance by leaving it some time to complete.

- You can clear the map cache, either by deleting it or by emptying it of all files. The map cache
  is the `map/cache` folder in your user files. For Windows user, that is typically:

  ```
  %userprofile%\Documents\Paradox Interactive\Victoria II\map\cache
  ```

  If you are loading a mod, you may be using mod-specific user files instead in which case the map
  cache is a `<mod-path>/map/cache` folder inside your user files.

- Steam users may try verifying the integrity of game files, in case the map files were corrupted.
  (This does not compromise your settings or save files.) If Steam find files to correct, consider
  clearing the map cache before starting the game.

- Other users may try reinstalling the game & clearing the map cache.

The game crashes when processing flags
--------------------------------------

<sup>[Back to the table of contents](#toc)</sup>

Note that the logs and the flag cache are part of the [user files][].

Symptoms:

- the game is stuck or crashes on the initial loading screen when processing or loading flags
- the game is stuck or crashes on or soon after startup, and the last thing that the logs mention is
  related to flags

Fixes:

- Initialising the flag cache for the first time is an expensive operation. If the game is not
  crashing, make sure you are giving the operation a chance by leaving it some time to complete.

- You can clear the flag cache, either by deleting it or by emptying it of all files. The flag cache
  is the `gfx/flags` folder in your user files. For Windows user, that is typically:

  ```
  %userprofile%\Documents\Paradox Interactive\Victoria II\gfx\flags
  ```

  If you are loading a mod, you may be using mod-specific user files instead in which case the flag
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
