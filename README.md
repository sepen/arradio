# [arradio](/)

Fetch and play internet radio stations directly from the command line

![Last Commit](https://img.shields.io/github/last-commit/sepen/arradio)
![Repo Size](https://img.shields.io/github/repo-size/sepen/arradio)
![Code Size](https://img.shields.io/github/languages/code-size/sepen/arradio)
![Proudly Written in Bash](https://img.shields.io/badge/written%20in-bash-ff69b4)

<img width="80%" alt="arradio v0.5.1" src="https://github.com/user-attachments/assets/afe67a77-67a6-409d-a049-35bdc7accfc1">

## Features

- Play radio stations from the [SHOUTcast](https://directory.shoutcast.com/) and [SomaFM](https://somafm.com) directories.
- Manage a list of favorite radio stations.
- Use multiple audio players like [arradio-player](https://github.com/sepen/arradio-player), [mpv](https://mpv.io), [vlc](https://www.videolan.org) and [ffplay](https://ffmpeg.org/) to play internet radio stations.
- Optional UI ([fzf](https://github.com/junegunn/fzf) pseudo-user interface) with color theme support.
- **IPTV support** for playing TV streams from M3U playlists.


## Table of Contents

* [Requirements](#requirements)
* [Installation](#installation)
* [Upgrading arradio](#upgrading-arradio)
* [Usage](#usage)
  * [Configuration](#configuration)
  * [Examples](#examples)
* [Favorites](#favorites)
  * [Add a radio station](#add-a-radio-station)
  * [Remove a radio station](#remove-a-radio-station)
* [UI Mode](#ui-mode)
* [UI Themes](#ui-themes)
  * [Installing themes](#installing-themes)
  * [Select a theme](#select-a-theme)


## Requirements

- Common tools found on most UNIX systems: (_bash_, _cut_, _grep_, _sed_, _head_, _cat_)
- XML tools are required to parse URL responses from [SHOUTcast](https://directory.shoutcast.com/) and [SomaFM](https://somafm.com) directories
  - [xmllint](https://gitlab.gnome.org/GNOME/libxml2/-/wikis/home)
- External audio player. This program does not play URL streams directly, this is delegated to an external audio player that should be installed on the system. The following ones are detected automatically if installed.
  - [arradio-player](https://github.com/sepen/arradio-player)
  - [mpv](https://mpv.io)
  - [vlc](https://www.videolan.org)
  - [ffplay](https://ffmpeg.org/)
- (optional) External command for the User Interface: [fzf](https://github.com/junegunn/fzf)



## Installation

To install [arradio](/) paste that in a macOS Terminal or Linux shell prompt:
```sh
curl -fsSL https://raw.githubusercontent.com/sepen/arradio/master/arradio | bash -s install
```

* The one-liner command from above installs [arradio](/) to its default `$HOME/.arradio`
* It will place some files under that prefix, so you'll need to set your PATH like this `export PATH=$HOME/.arradio/bin:$PATH`
* The installation explains what it will do, and you will see all that information. Consider adding this line to your _~/.bashrc_ or _~/.bash_profile_ or make sure to export this _PATH_ before running [arradio](/). The installation explains what it will do.
* The one-liner installation method found on [arradio](/) uses Bash. Notably, zsh, fish, tcsh and csh will not work.


## Upgrading arradio

[arradio](/) is being actively developed, and you might want to upgrade it once in a while. Please follow the instruction below:
```shell
arradio upgrade
```


## Usage
```sh
arradio help
Usage:
  arradio [command] <flags>

Commands:
  install                  Install arradio itself
  upgrade                  Upgrade arradio itself
  list                     List top streams (enabled services only)
  search [string]          Search across enabled services
  play [stream-id]         Play to specified stream
  info [stream-id]         Show info for a stream
  fadd [stream-id]         Add to favorites
  fdel [stream-id]         Remove from favorites
  flist                    List favorites
  ui                       Start UI (fzf required)
  tlist                    List installed UI themes
  env                      Show environment variables
  version                  Show version
  help                     Show this help

Flags:
  -s, --services [csv]     Limit services (shoutcast,somafm,iptv)
  -l, --limit [num]        Output limit (default: 50)
  -o, --output [fmt]       Format simple|wide (default: simple)
  -p, --player [cmd]       Player command (default: mpv)
  -t, --theme  [name]      UI theme (default: basic)
  -b, --no-color           Disable colors
  -n, --no-cache           Disable cache
  -d, --debug              Debug messages
```

### Configuration

The configuration file, by default located at `$HOME/.arradio/config`, uses a simple `variable: value` format.
Lines starting with `#` are treated as comments.

Example:
```yaml
# ~/.arradio/config

player_cmd:     mpv --no-video    # Command used to play streams  
ui_theme:       retrowave         # Name of the UI theme 
output_limit:   200               # Maximum number of results to display  
output_filter:  wide              # Filter type for output  
no_cache:       1                 # Disable caching when set to 1 
```
NOTE: This file is not created by default, so if you need to make changes to the default values, consider creating this configuration file. You can grab an example from [here](arradio.config)

Configuration values can come from several sources:

- As an environment variable
- As a value in the config file
- As a command line optional flag

The previous order also indicates the order of precedence to take.
For example, to override the default UI theme:
```sh
export ARRADIO_UI_THEME=nord
arradio ui
```

The above can be also override by setting a value in the config file:
```yaml
# ~/.arradio/config

ui_theme: purple
```

Lastly all from above can be override as an optional flag through the command line:
```sh
arradio ui --theme molokai
```

### IPTV Support

IPTV support in **arradio** is controlled by the `enable_iptv` option in your config file (`~/.arradio/config`).
When enabled, the UI lists IPTV streams as another “service” (like SHOUTcast or SomaFM).

It expects **M3U playlists** (or compatible formats), which **arradio** parses to present channels.
Playback is handled by your configured player (`player_cmd`, e.g., `mpv`, `vlc`, etc) but make sure your `player_cmd` **does not include options that disable video output** (such as `--no-video`), otherwise IPTV channels won’t show video correctly.  


Example configuration enabling IPTV and pointing to an M3U playlist:
```yaml
# ~/.arradio/config

player_cmd:    mpv
output_limit:  500
enable_iptv:   1
iptv_url:      https://raw.githubusercontent.com/iptv-org/iptv/refs/heads/master/streams/es.m3u
```

### Examples

Look for radio stations with the words _rock_ and _metal_ and limit the list to only 5 stations with wide output format:
```sh
arradio search rock -l 5

 STATION  GENRE               NAME
99498012  Rock                ROCK ANTENNE
99497966  Heavy Metal         ROCK ANTENNE Heavy Metal (Germany)
99497948  80s                 ANTENNE BAYERN 80er Hits
 1542116  Pop                 POWERHITZ.COM - THE OFFICEMIX
99497950  Rock                ANTENNE BAYERN Classic Rock
```

To play a radio station then use the `stream-id` from first column:
```sh
arradio play 99498012
```

## Favorites

By default this is empty but you can mantain a list of favorite radio stations.

To list my favourites:
```sh
arradio flist

 STATION  GENRE               NAME
 1210771  Funk                GENERATION SOUL DISCO FUNK RADIO [HD]
 1340450  Misc                AlienWare
 1528122  Jazz                JAZZGROOVE.org - The Jazz Groove
 1862204  Drum and Bass       DnBRadio.com
99497996  Pop                 ANTENNE BAYERN
99500354  Classic Rock        Classic Rock 109 - True Classic Rock!
99504568  Downtempo           Nordic Lodge - Copenhagen
99518870  Salsa               RADIO PANAMERICANA WEB
99540705  Rock                PureRock.US - America Pure Rock
99568323  Dance               Dance Wave Retro!
99571797  Techno              Minimal Mix Radio
99576960  Reggae              Roots Legacy Radio
```

### Add a radio station

To add a radio station you can do it in several ways.

Maybe you already have a `stream-id` from a previous search. In this case just run something like:
```sh
arradio fadd 99498012
```

You can also add radio stations from other locations. All you need is a valid stream URL. For that just create a new file under favorites directory like that:
```sh
cat > $HOME/.arradio/favorites/downtuned << __EOF__
id: downtuned
br: 128
genre: Rock
info: Groovy Music Sanctuary (https://www.downtunedmag.com)
url: http://195.242.237.14:8020/stream
__EOF__
```

### Remove a radio station

Similar to adding a `stream-id` to favorites, you can remove it with something like:
```sh
arradio fdel 99498012
```


## UI mode

[arradio](/) can run in pseudo-terminal User Interface mode. You just need to have _fzf_ installed and then you can run the following command to start UI mode:
```sh
arradio ui
```


## UI themes

[arradio](/) can perfectly work in UI mode without any theme installed. In this case it will use a black and white interface. But maybe you prefer to use a theme with fancy colors.

The themes are provided separately and their installation and upgrade will be manual.
To see the current themes available in this repository go to [ui-themes](ui-themes/).

Most themes also have a 256-color (8-bit) variant for terminals that do not support 24-bit truecolor. These 8-bit variants have names ending with 8. For example, retrowave has the 8-bit variant retrowave8.

You can use any theme, but for optimal appearance, choose the 24-bit version if your terminal supports truecolor; otherwise, use the 8-bit variant.

| Name | Screenshot | Description |
|------|------------|-------------|
| adwaita | <img src="https://github.com/user-attachments/assets/8df78fc5-78d9-4fdf-80f5-13fa94bb5fbe" width="640" style="height:auto;" /> | Dark foundation with calm blues, natural greens, and gentle yellow accents |
| basic | <img src='https://user-images.githubusercontent.com/11802175/283218989-c880d89c-6e04-4e9d-a13e-f8f983241db7.png' width="640" style="height:auto;" /> | Simple, portable color scheme optimized for compatibility with 256-color terminals |
| chalkboard | <img src="https://github.com/user-attachments/assets/e31b7ce8-8cc6-4cbb-90ae-685802e0af7c" width="640" style="height:auto;" /> | Dark board-like background with off-white text and subtle neon accents |
| dracula | <img src='https://github.com/user-attachments/assets/79496a91-9b09-4874-99ea-15490ad04892' width="640" style="height:auto;" /> | Dark, high-contrast palette of purples, pinks, and teals for vivid clarity |
| gruvbox | <img src='https://user-images.githubusercontent.com/11802175/283219015-c3435a75-1ca5-49c6-bbb3-fcca8738eff7.png' width="640" style="height:auto;" /> | Warm retro palette of earthy browns, oranges, and yellows based on morhetz/gruvbox |
| molokai | <img src='https://user-images.githubusercontent.com/11802175/283219007-36ff52e3-1f5d-4e46-90fc-f803eb49d188.png' width="640" style="height:auto;" /> | Focused dark palette with vibrant accents for coding or browsing based on tomasr/molokai |
| neoninferno | <img src="https://github.com/user-attachments/assets/212198a2-7af4-44e0-b9d5-73a9fee9137c" width="640" style="height:auto;" /> | Blazing neon accents over a subtle charcoal-purple background |
| nord | <img src='https://user-images.githubusercontent.com/11802175/283219010-0cb78f39-cad7-4949-890c-8e15c68eb197.png' width="640" style="height:auto;" /> | Cool, north-inspired tones of blue, gray, and icy white based on arcticicestudio/nord-vim |
| partypop | <img width="640" alt="Image" src="https://github.com/user-attachments/assets/9f745c67-250a-4260-b5bf-fd466df83631" width="640" style="height:auto;" /> | Ultra-bright, festive colors with clear contrasts for a playful vibe |
| purple | <img src='https://user-images.githubusercontent.com/11802175/283218996-50d6c815-d751-465f-9765-18c6f1c34d7b.png' width="640" style="height:auto;" /> | Deep purple scheme with soft highlights for a moody yet elegant feel |
| rasta | <img width="640" alt="Image" src="https://github.com/user-attachments/assets/5a52b964-fca4-4636-8ffe-65db05df04d1" width="640" style="height:auto;" /> | Bold red, gold, and green on a dark base for reggae-inspired energy |
| retroblast |<img width="640" alt="Image" src="https://github.com/user-attachments/assets/9c983a62-8266-439e-a713-a670876085f0" width="640" style="height:auto;" /> | Bright primaries and neon tones evoking ZX Spectrum and arcade classics |
| retrowave | <img src='https://github.com/user-attachments/assets/345bdee3-e06d-4a32-aa0f-82675b7434c1' width="640" style="height:auto;" /> | Deep blue background with neon pinks and cyan highlights for synthwave feels |
| seoul | <img src='https://user-images.githubusercontent.com/11802175/283219004-e6705984-fd13-44d8-bcfd-c29e5ced3ee5.png' width="640" style="height:auto;" /> | Light, balanced scheme with subtle contrasts for clean readability based on junegunn/seoul256.vim |
| seouldark | <img src='https://user-images.githubusercontent.com/11802175/283219001-b51d6b5b-5af5-4f59-b272-58f26c2eebff.png' width="640" style="height:auto;" > | Dark, understated scheme with smooth contrasts for extended use based on junegunn/seoul256.vim |


### Installing themes

This is an example about installing some themes:
```sh
url="https://raw.githubusercontent.com/sepen/arradio/master/ui-themes"
themes="dracula neoninferno partypop"
cd $HOME/.arradio/ui-themes
for theme in $themes; do curl -fsSL -O $url/$theme; done
cd -
```

To list all available themes locally, run:
```sh
arradio tlist

THEME            PALETTE  DESCRIPTION
basic              8-bit  Simple, portable color scheme optimized for compatibility with 256-color terminals
chalkboard        24-bit  Dark board-like background with off-white text and subtle neon accents
dracula           24-bit  Dark, high-contrast palette of purples, pinks, and teals for vivid clarity
neoninferno       24-bit  Blazing neon accents over a subtle charcoal-purple background
partypop          24-bit  Ultra-bright, festive colors with clear contrasts for a playful vibe
rasta             24-bit  Bold red, gold, and green on a dark base for reggae-inspired energy
retroblast        24-bit  Bright primaries and neon tones evoking ZX Spectrum and arcade classics
retrowave         24-bit  Deep blue background with neon pinks and cyan highlights for synthwave feels
```

### Selecting a Theme

By default, [arradio](/) reads the default theme from your configuration file:
```yaml
# ~/.arradio/config

ui_theme: gruvbox
```

You can also temporarily override the theme directly from the command line when launching arradio:
```sh
arradio ui -t gruvbox
```
