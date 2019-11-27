# arradio
CLI for listening ShoutCAST radio stations

## Installation ##
Paste that at a Terminal prompt. By default arradio will be installed in _/usr/local/bin_:
```
curl -fsSL https://raw.githubusercontent.com/sepen/arradio/master/install | bash
```
You can use an alternate directory for installation, e.g: _$HOME/bin_:
```
curl -fsSL https://raw.githubusercontent.com/sepen/arradio/master/install | INSTALL_DIR=$HOME/bin bash
```

## Usage ##
```
Usage: arradio <command> [options]
Where commands are:
  top500              Get top500 radio stations
  random              Get random list of radio stations
  search  <string>    Search for radio stations by keyword
  listen  <id>        Listen specified StationID
  fadd    <id>        Add radio station to your favourites
  fdel    <id>        Delete radio station from your favourites
  flist               List favourites radio stations
  help                Show this help information
  version             Show version information
Options available:
  -l <number>  Limit the number of stations to search and display
  -w           Wide output format
```

## Example ##

I want to find radio stations with the words _smooth_ and _jazz_.
I limit the list to only 2 stations.
I want wide output format.
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

Now I want to listen to the radio station. To do this I use the value of _StationID_.
```
$ arradio listen 1541073
```

I like it, so I add it to my favorites.
```
$ arradio fadd 1541073
```

I do not like it anymore. I remove it from my favorites.
```
$ arradio flist
 1541073 Smooth Jazz CD101.9 New York 64K
```
