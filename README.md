# `arradio`

Listen to internet radio stations from the terminal.

![Last Commit](https://img.shields.io/github/last-commit/sepen/arradio)
![Repo Size](https://img.shields.io/github/repo-size/sepen/arradio)
![Code Size](https://img.shields.io/github/languages/code-size/sepen/arradio)
![Proudly Written in Bash](https://img.shields.io/badge/written%20in-bash-ff69b4)

<img src="https://user-images.githubusercontent.com/11802175/280811770-c1db1da9-f392-4cd9-b954-8869cfe5e64a.png">

## Features

- Gets radio stations from the [SHOUTcast](https://directory.shoutcast.com/) directory
- Manage a list of favorite radio stations
- Use multiple audio players ([mpv](https://mpv.io), [vlc](https://www.videolan.org), [ffplay](https://ffmpeg.org/), etc.) to play radio stations
- Optional pseudo-user interface for terminal based on [fzf](https://github.com/junegunn/fzf) with support for 8-bit and 24-bit color themes.

## Table of Contents

* [Requirements](#requirements)
* [Installation](#installation)
* [Upgrading arradio](#upgrading-arradio)
* [Usage](#usage)
  * [Examples](#examples)
* [UI Mode](#ui-mode)
* [UI Themes](#ui-themes)
  * [Installing a theme](#installing-themes)
  * [Create a new theme](#create-a-new-theme)

## Requirements

- Common tools found on most UNIX systems: (_bash_, _cut_, _grep_, _sed_, _head_, _cat_)
- XML tools are required to parse URL responses from [SHOUTcast](https://directory.shoutcast.com/) directory
  - [xmllint](https://gitlab.gnome.org/GNOME/libxml2/-/wikis/home)
- External audio player. This program does not play URL streams directly, this is delegated to an external audio player that should be installed on the system. The following ones are detected automatically if installed.
  - [mpv](https://mpv.io)
  - [vlc](https://www.videolan.org)
  - [ffplay](https://ffmpeg.org/)
  - [arradio-player](https://github.com/sepen/arradio-player)
- (optional) External fuzzy finder for the User Interface:
  - [fzf](https://github.com/junegunn/fzf) is installed

![demo](demo/arradio.gif)

---

## Installation

To install **arradio** paste that in a macOS Terminal or Linux shell prompt:
```shell
curl -fsSL https://raw.githubusercontent.com/sepen/arradio/master/arradio | bash -s install
```

The one-liner command from above installs **arradio** to its default, `$HOME/.arradio` and will place some files under that prefix, so you'll need to set your PATH like this `export PATH=$HOME/.arradio/bin:$PATH`. \
The installation explains what it will do, and you will see all that information. Consider adding this line to your _~/.bashrc_ or _~/.bash_profile_ or make sure to export this _PATH_ before running **arradio**. The installation explains what it will do. \
The one-liner installation method found on **arradio** uses Bash. Notably, zsh, fish, tcsh and csh will not work.

## Upgrading arradio

`arradio` is being actively developed, and you might want to upgrade it once in a while. Please follow the instruction below:
```shell
arradio upgrade
```

## Usage
```
Usage:
  arradio [command] <flags>

Available Commands:
  install                   Install arradio itself
  upgrade                   Upgrade arradio itself
  top500                    Get top 500 radio stations
  search [string]           Search for radio stations by keyword
  listen [number]           Listen to specified radio station
  fadd [number]             Add radio station to your favourites
  fdel [number]             Delete radio station from your favourites
  flist                     List favourites radio stations
  ui                        Start arradio in UI mode (User Interface)
  themes                    List installed UI themes
  help                      Show this help information
  version                   Show version information

Optional Flags:
  -l, --limit [number]      Limit output lines (default: 20)
  -o, --output [string]     Output list format simple or wide (default: simple)
  -p, --player [string]     Command to play the streams. Default: arradio-player
  -t, --theme [string]      UI theme (default: basic)
  -n, --no-cache            Do not use cached resources
  -d, --debug               Enable debug messages
```

---

## Examples

Look for radio stations with the words _rock_ and _metal_ and limit the list to only 5 stations with wide output format:
```sh
arradio search rock -l 5 -o wide
 STATION  GENRE               NAME                                    INFO
99498012  Rock                ROCK ANTENNE                            Creedence Clearwater Revival - Bad moon rising
99497966  Heavy Metal         ROCK ANTENNE Heavy Metal (Germany)      Nightwish - Amaranth
99497948  80s                 ANTENNE BAYERN 80er Hits                Donna Summer - On the radio
 1542116  Pop                 POWERHITZ.COM - THE OFFICEMIX - The Be  
99497950  Rock                ANTENNE BAYERN Classic Rock             Steve Miller Band - Abracadabra
```

To play a radio station the use the STATION id from first column:
```sh
arradio listen 99498012
```

To add it to my favorites:
```sh
arradio fadd 99498012
```

To list my favourites:
```sh
arradio flist -o wide
 STATION  GENRE               NAME                                    INFO
99498012  Rock                ROCK ANTENNE                            Creedence Clearwater Revival - Bad moon rising
```

To remove it from my favorites:
```sh
arradio fdel 99498012
```

## UI mode

`arradio` can run in pseudo-terminal User Interface mode. You just need to have _fzf_ installed and then you can run the following command to start UI mode:
```sh
arradio ui
```

## UI themes

`arradio` can perfectly work in UI mode without any theme installed. It will use a basic black and white interface. But maybe you prefer to use a theme with fancy colors.

The themes are provided separately and their installation and upgrade will be manual.
To see the current themes available in this repository go to [ui-themes](ui-themes/).

You can use any of them, but keep in mind if your terminal supports 24-bit or truecolor, if not, it will be better to use the themes for 8-bit colors

List installed themes
```sh
arradio themes
```

Installing themes
```sh
curl -o ~/.arradio/ui-themes/molokai -s https://raw.githubusercontent.com/sepen/arradio/master/ui-themes/molokai
```

Use a theme in UI mode:
```sh
arradio ui -t molokai
```
