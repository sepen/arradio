<img src="https://github.com/sepen/arradio/assets/11802175/5a96278b-19ff-4e06-8871-846adb48d3fd" width="200">

# `arradio`

Listen to internet radio stations from the terminal.

![Last Commit](https://img.shields.io/github/last-commit/sepen/arradio)
![Repo Size](https://img.shields.io/github/repo-size/sepen/arradio)
![Code Size](https://img.shields.io/github/languages/code-size/sepen/arradio)
![Proudly Written in Bash](https://img.shields.io/badge/written%20in-bash-ff69b4)


## Features

* Gets radio stations from the [SHOUTcast](https://directory.shoutcast.com/) directory
* Manage a list of favorite radio stations
* Use multiple audio players ([mpv](https://mpv.io), [vlc](https://www.videolan.org), [ffplay](https://ffmpeg.org/), etc.) to play radio stations
* Optional pseudo-user interface for terminal based on [fzf](https://github.com/junegunn/fzf) with support for 8-bit and 24-bit color themes.

## Requirements

* Basic tools found on most UNIX systems: _cut_, _grep_, _sed_, _head_, _cat_
* XML tools are used to parse URL responses from [SHOUTcast](https://directory.shoutcast.com/) directory: [xmllint](https://gitlab.gnome.org/GNOME/libxml2/-/wikis/home)
* This program does not play streams directly, this is delegated to [mpv](https://mpv.io), [vlc](https://www.videolan.org), [ffplay](https://ffmpeg.org/), etc. program, so one of these programs must be previously installed.
* User Interface mode enabled when detected that [fzf](https://github.com/junegunn/fzf) is installed

![demo](demo/arradio.gif)

---

## Installation

To install **arradio** paste that in a macOS Terminal or Linux shell prompt:
```
$ curl -fsSL https://raw.githubusercontent.com/sepen/arradio/master/arradio | bash -s install
```

The one-liner command from above installs **arradio** to its default, `$HOME/.arradio` and will place some files under that prefix, so you'll need to set your PATH like this `export PATH=$HOME/.arradio/bin:$PATH`. \
The installation explains what it will do, and you will see all that information. Consider adding this line to your _~/.bashrc_ or _~/.bash_profile_ or make sure to export this _PATH_ before running **arradio**. The installation explains what it will do. \
The one-liner installation method found on **arradio** uses Bash. Notably, zsh, fish, tcsh and csh will not work.

---

## Usage
```
Usage:
  arradio [command] <flags>

Available Commands:
  install                   Install arradio itself
  update                    Update arradio itself
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

I want to find radio stations with the words _smooth_ and _jazz_. I limit the list to only 2 stations _(-l 2)_ and I want wide output format _(-w)_.
```
$ arradio search smooth jazz -l 2 -o wide
 STATION  GENRE               NAME                                    INFO
99426190  Smooth Jazz         BEST SMOOTH JAZZ - UK (LONDON) HOST RO  Host Rod Lucas - Londons Best Smooth Jazz UK
 1852944  Smooth Jazz         1.FM - Bay Smooth Jazz (www.1.fm)
```

Now I want to listen the second radio station in the list. To do this I use the value of _STATION_.
```
$ arradio listen 1852944
```

I like it, so I add it to my favorites.
```
$ arradio fadd 1852944
```

I want to list my favourites.
```
$ arradio flist
 STATION  GENRE               NAME
 1852944  Smooth Jazz         1.FM - Bay Smooth Jazz (www.1.fm)
```

To remove it from my favorites.
```
$ arradio fdel 1852944
```
