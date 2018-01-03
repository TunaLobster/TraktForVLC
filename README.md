TraktForVLC [![Travis Build Status](https://travis-ci.org/XaF/TraktForVLC.svg?branch=master)](https://travis-ci.org/XaF/TraktForVLC) [![AppVeyor Build status](https://ci.appveyor.com/api/projects/status/e1ie51bwhbki60ns/branch/master?svg=true)](https://ci.appveyor.com/project/XaF/traktforvlc/branch/master)
===========

## Table of Contents

  * [Presentation](#presentation)
  * [Information](#information)
    * [Credits](#credits)
    * [Licence](#licence)
  * [Installation](#installation)
    * [Compatibility](#compatibility)
      * [Operating System](#operating-system)
      * [Python version](#python-version)
      * [VLC version](#vlc-version)
    * [Getting sources](#getting-sources)
  * [Configuration](#configuration)
    * [Configuring VLC](#configuring-vlc)
      * [With VLC Settings](#with-vlc-settings)
      * [With command line](#with-command-line)
      * [Automatic configuration on Windows (without guarantee)](#automatic-configuration-on-windows-without-guarantee)
    * [Configuring TraktForVLC](#configuring-traktforvlc)
  * [Automatic start](#automatic-start)
    * [On Linux](#on-linux)
      * [Using Gnome](#using-gnome)
      * [Using <code>.bash_profile</code>](#using-bash-profile)
    * [On Windows](#on-windows)
      * [Using scheduled tasks](#using-scheduled-tasks)
    * [On Mac OS X](#on-mac-os-x)
      * [Using a <code>.plist</code> file](#using-a-plist-file)
  * [Issues](#issues)

## Presentation

TraktForVLC allows scrobbling [VLC](https://videolan.org/) content to [Trakt](https://trakt.tv/). Since there is no way for VLC to directly know if you are watching a TV show or a Movie, this script will attempt to figure it out using the name of the video and its length. It is therefore possible that the script can't find what you're watching if your files are not named properly.


## Information
### Credits

The TraktForVLC script is originally based off of [TraktForBoxee](https://github.com/cold12/Trakt-for-Boxee) and works very similarly to it.

This version of the script is forked from [Wifsimster's TraktForVLC](https://github.com/Wifsimster/TraktForVLC), itself based on the original [quietcore's TraktForVLC](https://github.com/quietcore/TraktForVLC).

### Licence

Copyright (C) 2012       Chris Maclellan <<chrismaclellan@gmail.com>>

Copyright (C) 2013       Damien Battistella <<wifsimster@gmail.com>>

Copyright (C) 2014-2015  Raphaël Beamonte <<raphael.beamonte@gmail.com>>

This program is free software; you can redistribute it and/or
modify it under the terms of the GNU General Public License
as published by the Free Software Foundation; either version 2
of the License, or (at your option) any later version.

This program is distributed in the hope that it will be useful,
but WITHOUT ANY WARRANTY; without even the implied warranty of
MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  [See the
GNU General Public License for more details](https://www.gnu.org/licenses/gpl-2.0.html).


## Installation
### Compatibility
#### Operating System
As I'm a fervent [Linux](https://www.kernel.org/) user, this version of the script has only been used and tested on Linux [Debian](https://www.debian.org/). I will not be able to maintain directly the [Windows](http://windows.microsoft.com) compatibility but I'm open to pull requests on this side, as we're here to share.

#### Python version
This version of TraktForVLC has been tested using Python 2.7.9 and Python 3.4.2 It should however be compatible with the following versions:
+ Python 2.6
+ Python 2.7
+ Python 3.3
+ Python 3.4

#### VLC version
This version of TraktForVLC has been tested using VLC 2.2.0 Weatherwax

### Getting sources
To get the sources, you only need to clone the repository using the following command on Linux:

```sh
git clone https://github.com/XaF/TraktForVLC.git
```

On Windows, you can download the [last version of the ZIP package](https://github.com/XaF/TraktForVLC/archive/master.zip).

## Configuration
### Configuring VLC
#### With VLC Settings
Go to:
+ **Tools**
+ then **Preferences**

In the preferences window that just opened, look at the bottom left and check **All**.

Then, in the menu that appeared in place of the icons on the left, go to:
+ **Interface**
+ **Main interfaces**

Here, in the list of **Extra interface modules**, check **Remote control interface**.

> **WARNING** in some versions of VLC, when checking **Remote control interface**,
> you will see **oldrc** appearing in the lines below. If it is the case, please
> edit manually that line to read **rc** instead. TraktForVLC can use **oldrc**,
> but the behavior of **rc** is much more predictable thus preferable.

Then, go to:
+ **RC** (submenu of **Main interfaces**)

And in the line **TCP command input** insert *localhost:4222* (you can adjust the host and port if you want, but you'll have to keep it in mind for the next steps).

On Windows, you will also find a checkbox **Do not open a DOS command box interface** that you should check if you don't want to see a console each time you start VLC.

#### With command line
Instead of configuring VLC, you can use parameters in your VLC command line to start the remote control server. The following line can be use either way on Linux or Windows by adjusting the `vlc` at start by the path to your VLC executable (usually `vlc.exe` on Windows).

```sh
vlc --extraintf=rc --rc-host=localhost:4222
```

On Windows, you can also add the ```--rc-quiet``` option to disable the console.

#### Automatic configuration on Windows (without guarantee)

The ```windows_batch/``` directory of this repository contains different batch files. The file ```set_registry_keys.bat``` should allow to automatically configure your system to open some multimedia files with VLC and with the right options.

The ```config.bat``` file allows to configure which multimedia files are
concerned, and other options needed before running the ```set_registry_keys.bat```:

```batch
:::: Configuration to set the registry keys
:: Set the full path to the VLC exe file on your computer
set vlc_path=C:\your\path\to\VideoLAN\VLC\vlc.exe

:: Set the IP address VLC will listen to (default: localhost)
set ip=localhost

:: Set the port VLC will listen to (default: 4222)
set port=4222

:: Set the file formats for which you want VLC to be start
:: listening to remote control for TraktForVLC to work. You
:: can add as much format as you want, all format must be
:: separated by a coma.
set formats=avi, mkv, mov, mp4, wmv, ts, mpg
```

Configure those options carefully, then run (double-click for instance) the
```set_registry_keys.bat``` file. VLC should then be ready, you can now
[configure TraktForVLC](#configuring-traktforvlc).

### Configuring TraktForVLC

TraktForVLC needs its `config.ini` file to work properly. The content of the file is as follow:

```yaml
[VLC]
IP = localhost              # The host of the VLC Remote Control server
Port = 4222                 # The port of the VLC Remote Control server
```

This is the general server configuration. You should put here the same information you used in your configuration of VLC to allow TraktForVLC to connect to your VLC instance.

```yaml
[Trakt]
PIN = [pin code]            # Identify yourself on the page https://trakt.tv/pin/2498
			                # and put here the resulting pin code
```

This is a [Trakt pin you can get from this web page](https://trakt.tv/pin/2498). After the first Trakt connection, this ```PIN``` entry will be replaced automatically by two new entries: ```access_token``` and ```refresh_token```. Be careful not to remove those entries from your configuration, or you will need to put back a ```PIN``` entry with a new freshly generated pin code from trakt.tv.

```yaml
[TraktForVLC]
Timer = 60                  # Time (sec) between each loop and look of TraktForVLC
StartWatching = 30          # Time (sec) before considering a video is currently watched
UseFilenames = No           # Whether or not to use filenames instead of VLC window title
ScrobblePercent = 90        # Percentage (%) of the video to be spent before scrobbling it
ScrobbleMovie = Yes         # Whether or not TraktForVLC will automatically scrobble movies
ScrobbleTV = Yes            # Whether or not TraktForVLC will automatically scrobble tv shows
WatchingMovie = Yes         # Whether or not movies will be marked as being watched
WatchingTV = Yes            # Whether or not tv shows will be marked as being watched
```
This is the script-specific configuration, the time between each loop of
work, the time before considering you're effectively watching a video, and
whether or not scrobble automatically what you're watching on Trakt.

## Automatic start
This script needs to be kept alive, and to be working when you start watching something. Achieving this need is very different if you're on Linux or Windows. This part aims to guide you to do these steps.

### On Linux
There is different way to realize what we want to do on Linux. I'll just give here two different and simple ways as reference.

#### Using Gnome
Go to your **Startup Applications Preferences**, then click on the **Add** button. Here, you can give the name you want to your task, and you only need to add the following line in the **Command** box:
```sh
path/to/your/TraktForVLC.py --daemon
```

Your task will then automatically be started with your session.

#### Using ```.bash_profile```
If you want to start Trakt automatically with your session but don't want to use Gnome's **Startup Applications Preferences**, you can add the line proposed below directly in your `.bash_profile` script.

### On Windows
#### Using scheduled tasks
A `start_process.bat` file is joined to the source files of TraktForVLC. You need to edit it to reflect the data and configuration directory of your TraktForVLC installation, then you can add this program as a scheduled task that starts when you open your session.

### On Mac OS X
#### Using a ```.plist``` file
Save the following file as ```org.TraktForVLC.plist``` in your ```~/Library/LaunchAgents/``` folder (you can copy and edit the file from the ```macosx``` directory of this repository):
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN"
    "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
    <key>Label</key>
    <string>org.user.TraktForVLC</string>
    <key>ProgramArguments</key>
    <array>
        <string>[EDIT: /usr/bin/python]</string>
        <string>[EDIT: /path/to/TraktForVLC/TraktForVLC.py]</string>
    </array>
    <key>WorkingDirectory</key>
    <string>[EDIT: /path/to/TraktForVLC]</string>
    <key>RunAtLoad</key>
    <true />
    <key>LaunchOnlyOnce</key>
    <true />
    <key>KeepAlive</key>
    <true />
</dict>
</plist>
```

And edit the ```[EDIT: ...]``` blocks to reflect your installation.

You can then load the file using the following command:
```sh
launchctl load ~/Library/LaunchAgents/org.TraktForVLC.plist
```

```TraktForVLC``` should be started as soon as you enter that last command,
and will automatically be started for subsequent system boots.

## Issues
Please use the [GitHub integrated issue tracker](https://github.com/XaF/TraktForVLC/issues) for every problem you can encounter. Please **DO NOT** use my email for issues or walkthrough.

When submitting an issue, please submit a TraktForVLC logfile showing the error. You can start TraktForVLC in debug mode (```--debug``` option) to obtain more thorough logs and with small timers (```--small-timers``` option) to speed up its internal loop. The resulting logfile (```TraktForVLC-DEBUG.log```) should then be available in your ```logs/``` directory.

> **Please** be careful to remove any personnal information that could still be in the logfile (password, identification token, ...) before putting your file online.
