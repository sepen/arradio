<img src="https://github.com/sepen/arradio/assets/11802175/5a96278b-19ff-4e06-8871-846adb48d3fd" width="200">

# `arradio`

Listen to internet radio stations from the terminal.

![Last Commit](https://img.shields.io/github/last-commit/sepen/k8kreator)
![Repo Size](https://img.shields.io/github/repo-size/sepen/k8kreator)
![Code Size](https://img.shields.io/github/languages/code-size/sepen/k8kreator)
![Proudly Written in Bash](https://img.shields.io/badge/written%20in-bash-ff69b4)

The main utility of _arradio_ is to search for radio stations in the ShoutCAST directory, save the favorite radio stations for later and avoid manual link management to the streaming url for every time you want to listen to a radio station.

This program does not play streams directly, this is delegated to _mpv_, _mplayer_ or _vlc_ program, so one of these programs must be previously installed.

![demo](demo/arradio.gif)

---

## Installation

To install **arradio** paste that in a macOS Terminal or Linux shell prompt:
```
$ curl -fsSL https://raw.githubusercontent.com/sepen/arradio/master/arradio | bash -s self-install
```

The one-liner command from above installs **arradio** to its default, `$HOME/.arradio` and will place some files under that prefix, so you'll need to set your PATH like this `export PATH=$HOME/.arradio/bin:$PATH`. \
The installation explains what it will do, and you will see all that information. Consider adding this line to your _~/.bashrc_ or _~/.bash_profile_ or make sure to export this _PATH_ before running **arradio**. The installation explains what it will do. \
The one-liner installation method found on **arradio** uses Bash. Notably, zsh, fish, tcsh and csh will not work.

---

## Usage
```
Usage:
  arradio [command]

Available Commands:
  top500             Get arradio-top500 radio stations
  random             Get random list of radio stations
  search  string     Search for radio stations by keyword
  listen  id         Listen specified StationSHOUTCAST_API_ID
  fadd    id         Add radio station to your favourites
  fdel    id         Delete radio station from your favourites
  flist              List favourites radio stations
  help               Show this help information
  version            Show version information
  self-install       Install arradio itself
  self-update        Update arradio itself

Flags::
  -l number          Limit the number of stations to search and display
  -w                 Wide output format
```

---

## Examples

I want to find radio stations with the words _smooth_ and _jazz_. I limit the list to only 2 stations _(-l 2)_ and I want wide output format _(-w)_.
```
$ arradio search smooth jazz -l 2 -w
StationName:	SmoothJazz.com Global
MediaType:	audio/mpeg
StationID:	1477271
Bitrate:	128
Genre:		Smooth Jazz genre2=Jazz logo=http://i.radionomy.com/document/radios/7/7d37/7d37f56b-67ff-40b7-9102-bc15172d7831.jpg

StationName:	Smooth Jazz CD101.9 New York 64K
MediaType:	audio/mpeg
StationID:	1541073
Bitrate:	128
Genre:		Smooth Jazz genre2=Acid Jazz genre3=Jazz genre4=Easy Listening genre5=Jazz logo=http://i.radionomy.com/document/radios/d/d8d7/d8d7a5f6-df88-4bd9-b5b2-e02b4fcaf661.jpg
Playing:	Brian Simpson - Out Of A Dream (Featuring Najee)
```

Now I want to listen the second radio station in the list. To do this I use the value of _StationID_.
```
$ arradio listen 1541073
```

I like it, so I add it to my favorites.
```
$ arradio fadd 1541073
```

I want to list my favourites.
```
$ arradio flist
 1541073 Smooth Jazz CD101.9 New York 64K
```

I do not like it anymore. I remove it from my favorites.
```
$ arradio fdel 1541073
```
