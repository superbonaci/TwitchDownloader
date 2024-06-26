# TwitchDownloaderCLI
A cross platform command line tool that can do the main functions of the GUI program, which can download VODs/Clips/Chats and render chats.
Also can concatenate/combine/merge Transport Stream files, either those parts downloaded with the CLI itself or from another source.

- [TwitchDownloaderCLI](#twitchdownloadercli)
  - [Arguments for mode videodownload](#arguments-for-mode-videodownload)
  - [Arguments for mode clipdownload](#arguments-for-mode-clipdownload)
  - [Arguments for mode chatdownload](#arguments-for-mode-chatdownload)
  - [Arguments for mode chatupdate](#arguments-for-mode-chatupdate)
  - [Arguments for mode chatrender](#arguments-for-mode-chatrender)
  - [Arguments for mode ffmpeg](#arguments-for-mode-ffmpeg)
  - [Arguments for mode cache](#arguments-for-mode-cache)
  - [Arguments for mode tsmerge](#arguments-for-mode-tsmerge)
  - [Example Commands](#example-commands)
  - [Additional Notes](#additional-notes)

### Global arguments

The mode in use must support also the global arguments which are passed to the CLI.

**--banner**
(Default: `true`) Displays a banner containing version and copyright information.

**--silent**
(Default: `false`) Suppresses progress console output. `--silent true` implies `--banner false`.

---

## Arguments for mode videodownload
#### Downloads a stream VOD or highlight from Twitch

**-u / --id (REQUIRED)**
The ID or URL of the VOD to download.

**-o / --output**
Path to output file. File extension will be used to determine download type. Valid extensions are: .mp4, .m4a.

**-q / --quality**
The quality the program will attempt to download, like '1080p60'. If '-o' and '-q' are missing will be 'best'.

**-p / --parts**
Only download playlist.m3u8, metadata.txt and .ts parts to cache folder, and exit. Overrides '-k', '-K', '-o'.

**-K / --cache**
Keep entire cache folder. Overrides '-k'.

**-k / --cache-noparts**
Keep cache folder except .ts parts. Merged 'output.ts' is not considered a part.

**-F / --skip-storagecheck**
Skip checking for free storage space.

**-b / --beginning**
Time in seconds where the crop of the ID begins. May break first GOP. For example if I had a 10 second stream but only wanted the last 7 seconds of it I would use `-b 3` to skip the first 3 seconds.

**-e / --ending**
Time in seconds where the crop of the ID ends. May break last GOP. For example if I had a 10 second stream but only wanted the first 4 seconds of it I would use `-e 4` to end on the 4th second.

Extra example, if I wanted only seconds 3-6 in a 10 second stream I would do `-b 3 -e 6`

**--tbn**
Set specific TBN (time base in AVStream) for output.

**-t / --threads**
(Default: `4`) Number of simultaneous download threads.

**--bandwidth**
(Default: `-1`) The maximum bandwidth a thread will be allowed to use in kibibytes per second (KiB/s), or `-1` for no maximum.

**-a / --oauth**
OAuth access token to download subscriber only VODs. <ins>**DO NOT SHARE YOUR OAUTH TOKEN WITH ANYONE.**</ins>

**--ffmpeg-path**
Path to FFmpeg executable.

**-c / --temp-path**
Set custom path to temp/cache folder instead of provided by system. Recommended for '-k', '-K', '-p'.

**--banner**
(Default: `true`) Displays a banner containing version and copyright information.

## Arguments for mode clipdownload
#### Downloads a clip from Twitch

**-u / --id (REQUIRED)**
The ID or URL of the Clip to download.

**-o / --output (REQUIRED)**
File the program will output to.

**-q / --quality**
The quality the program will attempt to download, for example "1080p60", if not found will download highest quality video.

**--bandwidth**
(Default: `-1`) The maximum bandwidth the clip downloader is allowed to use in kibibytes per second (KiB/s), or `-1` for no maximum.

**--encode-metadata**
(Default: `true`) Uses FFmpeg to add metadata to the clip output file.

**--tbn**
Set specific TBN (time base in AVStream) for output.

**--ffmpeg-path**
Path to FFmpeg executable.

**--temp-path**
Path to temporary folder for cache.

**--banner**
(Default: `true`) Displays a banner containing version and copyright information.

## Arguments for mode chatdownload
#### Downloads the chat of a VOD, highlight, or clip

**-u / --id (REQUIRED)**
The ID or URL of the VOD or clip to download.

**-o / --output (REQUIRED)**
File the program will output to. File extension will be used to determine download type. Valid extensions are: `.json`, `.html`, and `.txt`.

**--compression**
(Default: `None`) Compresses an output json chat file using a specified compression, usually resulting in 40-90% size reductions. Valid values are: `None`, `Gzip`. More formats will be supported in the future.

**-b / --beginning**
Time in seconds where the crop begins. For example if I had a 10 second stream but only wanted the last 7 seconds of it I would use `-b 3` to skip the first 3 seconds.

**-e / --ending**
Time in seconds where the crop ends. For example if I had a 10 second stream but only wanted the first 4 seconds of it I would use `-e 4` to end on the 4th second.

**-E / --embed-images**
(Default: `false`) Embed first party emotes, badges, and cheermotes into the download file for offline rendering. Useful for archival purposes, file size will be larger.

**--bttv**
(Default: `true`) BTTV emote embedding. Requires `-E / --embed-images`.

**--ffz**
(Default: `true`) FFZ emote embedding. Requires `-E / --embed-images`.

**--stv**
(Default: `true`) 7TV emote embedding. Requires `-E / --embed-images`.

**--timestamp-format**
(Default: `Relative`) Sets the timestamp format for .txt chat logs. Valid values are: `Utc`, `UtcFull`, `Relative`, and `None`.

**--chat-connections**
(Default: `4`) The number of parallel downloads for chat.

**--silent**
(Default: `false`) Suppresses progress console output.

**--temp-path**
Path to temporary folder for cache.

**--banner**
(Default: `true`) Displays a banner containing version and copyright information.

## Arguments for mode chatupdate
#### Updates the embedded emotes, badges, bits, and crops of a chat download and/or converts a JSON chat to another format

**-i / --input (REQUIRED)**
Path to input file. Valid extensions are: `.json`, `.json.gz`.

**-o / --output (REQUIRED)**
Path to output file. File extension will be used to determine new chat type. Valid extensions are: `.json`, `.html`, and `.txt`.

**-c / --compression**
(Default: `None`) Compresses an output json chat file using a specified compression, usually resulting in 40-90% size reductions. Valid values are: `None`, `Gzip`. More formats will be supported in the future.

**-E / --embed-missing**
(Default: `false`) Embed missing emotes, badges, and cheermotes. Already embedded images will be untouched.

**-R / --replace-embeds**
(Default: `false`) Replace all embedded emotes, badges, and cheermotes in the file. All embedded data will be overwritten!

**b / --beginning**
(Default: `-1`) New time in seconds where the chat begins. Comments may be added but not removed. -1 = No crop.

**-e / --ending**
(Default: `-1`) New time in seconds where chat ends. Comments may be added but not removed. -1 = No crop.

**--bttv**
(Default: `true`) Enable embedding BTTV emotes.

**--ffz**
(Default: `true`) Enable embedding FFZ emotes.

**--stv**
(Default: `true`) Enable embedding 7TV emotes.

**--timestamp-format**
(Default: `Relative`) Sets the timestamp format for .txt chat logs. Valid values are: `Utc`, `Relative`, and `None`.

**--temp-path**
Path to temporary folder for cache.

**--banner**
(Default: `true`) Displays a banner containing version and copyright information.

## Arguments for mode chatrender
#### Renders a chat JSON as a video

**-i / --input (REQUIRED)**
The path to the `.json` or `.json.gz` chat file input.

**-o / --output (REQUIRED)**
File the program will output to.

**--background-color**
(Default: `#111111`) The render background color in the string format of `#RRGGBB` or `#AARRGGBB` in hexadecimal.

**--alt-background-color**
(Default: `#191919`) The alternate message background color in the string format of `#RRGGBB` or `#AARRGGBB` in hexadecimal. Requires `--alternate-backgrounds`.

**--message-color**
(Default: `#ffffff`) The message text color in the string format of `#RRGGBB` or `#AARRGGBB` in hexadecimal.

**-w / --chat-width**
(Default: `350`) Width of chat render.

**-h / --chat-height**
(Default: `600`) Height of chat render.

**-b / --beginning**
(Default: `-1`) Time in seconds where the crop begins.

**-e / --ending**
(Default: `-1`) Time in seconds where the crop ends.

**--bttv**
(Default: `true`) Enable BTTV emotes.

**--ffz**
(Default: `true`) Enable FFZ emotes.

**--stv**
(Default: `true`) Enable 7TV emotes.

**--allow-unlisted-emotes**
(Default: `true`) Allow unlisted 7TV emotes in the render.

**--sub-messages**
(Default: `true`) Enable sub / re-sub messages.

**--badges**
(Default: `true`) Enable chat badges.

**--outline**
(Default: `false`) Enable outline around chat messages.

**--outline-size**
(Default: `4`) Size of outline if outline is enabled.

**-f / --font**
(Default: `Inter Embedded`) Font to use.

**--font-size**
(Default: `12`) Font size.

**--message-fontstyle**
(Default: `normal`) Font style of message. Valid values are **normal**, **bold**, and **italic**.

**--username-fontstyle**
(Default: `bold`) Font style of username. Valid values are **normal**, **bold**, and **italic**.

**--timestamp**
(Default: `false`) Enables timestamps to left of messages, similar to VOD chat on Twitch.

**--generate-mask**
(Default: `false`) Generates a mask file of the chat in addition to the rendered chat.

**--sharpening**
(Default: `false`) Appends `-filter_complex "smartblur=lr=1:ls=-1.0"` to the `input-args`. Works best with `font-size` 24 or larger.

**--framerate**
(Default: `30`) Framerate of the render.

**--update-rate**
(Default: `0.2`) Time in seconds to update chat render output.

**--input-args**
(Default: `-framerate {fps} -f rawvideo -analyzeduration {max_int} -probesize {max_int} -pix_fmt bgra -video_size {width}x{height} -i -`) Input arguments for FFmpeg chat render.

**--output-args**
(Default: `-c:v libx264 -preset veryfast -crf 18 -pix_fmt yuv420p "{save_path}"`) Output arguments for FFmpeg chat render.

**--ignore-users**
(Default: ` `) List of usernames to ignore when rendering, separated by commas. Not case-sensitive.

**--ban-words**
(Default: ` `) List of words or phrases to ignore when rendering, separated by commas. Not case-sensitive.

**--badge-filter**
(Default: `0`) Bitmask of types of Chat Badges to filter out. Add the numbers of the types of badges you want to filter. For example, to filter out Moderator and Broadcaster badges only enter the value of 6.

Other = `1`, Broadcaster = `2`, Moderator = `4`, VIP = `8`, Subscriber = `16`, Predictions = `32`, NoAudioVisual = `64`, PrimeGaming = `128`

**--dispersion**
(Default: `false`) In November 2022 a Twitch API change made chat messages download only in whole seconds. This option uses additional metadata to attempt to restore messages to when they were actually sent. This may result in a different comment order. Requires an update rate less than 1.0 for effective results.

**--alternate-backgrounds**
(Default: `false`) Alternates the background color of every other chat message to help tell them apart.

**--offline**
(Default: `false`) Render completely offline using only embedded emotes, badges, and bits from the input json.

**--emoji-vendor**
(Default: `notocolor`) The emoji vendor used for rendering emojis. Valid values are: `twitter` / `twemoji`, `google` / `notocolor`, `none`.

**--ffmpeg-path**
(Default: ` `) Path to FFmpeg executable.

**--temp-path**
(Default: ` `) Path to temporary folder for cache.

**--verbose-ffmpeg**
(Default: `false`) Prints every message from FFmpeg.

**--skip-drive-waiting**
(Default: `false`) Do not wait for the output drive to transmit a ready signal before writing the next frame. Waiting is usually only necessary on low-end USB drives. Skipping can result in 1-5% render speed increases.

**--scale-emote**
(Default: `1.0`) Number to scale emote images.

**--scale-badge**
(Default: `1.0`) Number to scale badge images.

**--scale-emoji**
(Default: `1.0`) Number to scale emoji images.

**--scale-vertical**
(Default: `1.0`) Number to scale vertical padding.

**--scale-side-padding**
(Default: `1.0`) Number to scale side padding.

**--scale-section-height**
(Default: `1.0`) Number to scale section height of comments.

**--scale-word-space**
(Default: `1.0`) Number to scale spacing between words.

**--scale-emote-space**
(Default: `1.0`) Number to scale spacing between emotes.

**--scale-highlight-stroke**
(Default: `1.0`) Number to scale highlight stroke size (sub messages).

**--scale-highlight-indent**
(Default: `1.0`) Number to scale highlight indent size (sub messages).

**--banner**
(Default: `true`) Displays a banner containing version and copyright information.


## Arguments for mode ffmpeg
#### Manage standalone FFmpeg

**-d / --download**
(Default: `false`) Downloads FFmpeg as a standalone file.

**--banner**
(Default: `true`) Displays a banner containing version and copyright information.

## Arguments for mode cache
#### Manage the working cache.

**-c / --clear**
(Default: `false`) Clears the default cache folder.

**--force-clear**
(Default: `false`) Clears the default cache folder, bypassing the confirmation prompt.

**--banner**
(Default: `true`) Displays a banner containing version and copyright information.

## Arguments for mode tsmerge
#### Concatenates multiple .ts/.tsv/.tsa/.m2t/.m2ts (MPEG Transport Stream) files into a single file

**-i / --input (REQUIRED)**
Path a text file containing the absolute paths of the files to concatenate, separated by newlines. M3U/M3U8 is also supported.

**-o / --output (REQUIRED)**
File the program will output to.

**-f / --ignore-missing**
(Default: `false`) Ignore missing files listed inside input. Useful when the stream was trimmed.

**--banner**
(Default: `true`) Displays a banner containing version and copyright information.

---

## Example Commands
#### Examples of typical TwitchDownloaderCLI use cases.

Note: Commands are formatted for unix systems (i.e. Mac, Linux). For usage on Windows, replace `./TwitchDownloaderCLI` with `TwitchDownloaderCLI.exe` (cmd) or `./TwitchDownloaderCLI.exe` (powershell).

Download a VOD with defaults

    ./TwitchDownloaderCLI videodownload --id 612942303 -o video.mp4

Download a Clip with defaults

    ./TwitchDownloaderCLI clipdownload --id NurturingCalmHamburgerVoHiYo -o clip.mp4

Download a Chat JSON with embedded emotes/badges from Twitch and emotes from Bttv

    ./TwitchDownloaderCLI chatdownload --id 612942303 --embed-images --bttv=true --ffz=false --stv=false -o chat.json

Download a Chat as plain text with timestamps

    ./TwitchDownloaderCLI chatdownload --id 612942303 --timestamp-format Relative -o chat.txt

Add embeds to a chat file that was downloaded without embeds

    ./TwitchDownloaderCLI chatupdate -i chat.json -o chat_embedded.json --embed-missing

Convert a JSON chat file to HTML

    ./TwitchDownloaderCLI chatupdate -i chat.json -o chat.html

Render a chat with defaults

    ./TwitchDownloaderCLI chatrender -i chat.json -o chat.mp4

Render a chat with custom video settings and message outlines

    ./TwitchDownloaderCLI chatrender -i chat.json -h 1440 -w 720 --framerate 60 --outline -o chat.mp4

Render a chat with custom FFmpeg arguments

    ./TwitchDownloaderCLI chatrender -i chat.json --output-args='-c:v libx264 -preset veryfast -crf 18 -pix_fmt yuv420p "{save_path}"' -o chat.mp4

Download a portable FFmpeg binary for your system

    ./TwitchDownloaderCLI ffmpeg --download

Clear the default TwitchDownloader cache folder

    ./TwitchDownloaderCLI cache --clear

Concatenate several ts files into a single output file

    TwitchDownloaderCLI tsmerge -i list.txt -o output.ts

Print the available operations

    ./TwitchDownloaderCLI help

Print the available options for the VOD downloader

    ./TwitchDownloaderCLI videodownload --help

---

## Additional Notes
#### General

All `--id` inputs will accept either video/clip IDs or full video/clip URLs. i.e. `--id 612942303`, `--id https://twitch.tv/videos/612942303` or `--id https://www.twitch.tv/videos/799499623?filter=all&sort=views`.

String arguments that contain spaces should be wrapped in either single quotes <kbd>'</kbd> or double quotes <kbd>"</kbd>. i.e. `--output 'my output file.mp4'` or `--output "my output file.mp4"`

Default true boolean flags must be assigned: `--default-true-flag=false`. Default false boolean flags should still be raised normally: `--default-false-flag`.

For Linux users, ensure both `fontconfig` and `libfontconfig1` are installed. `apt-get install fontconfig libfontconfig1` on Ubuntu.

Some distros, like Linux Alpine, lack fonts for some languages (Arabic, Persian, Thai, etc.) If this is the case for you, install additional fonts families such as [Noto](https://fonts.google.com/noto/specimen/Noto+Sans) or check your distro's wiki page on fonts as it may have an install command for this specific scenario, such as the [Linux Alpine](https://wiki.alpinelinux.org/wiki/Fonts) font page.

When cropping, the part of the file to be retained is the one after the crop starts and before the crop ends. The rest is discarded. So in this context 'crop' means 'crop in', while in others could mean 'crop out' and that's the opposite.

The location of the `temp`, `temporary` or `cache` folder, is automatically assigned by the operating system by default. In some operating systems can be difficult or impossible to access by a program different than `TwitchDownloader`, if it's in a private or encrypted area, or if the permissions are limited. Also could cause excessive wear of the internal flash storage. This is why it's recommended to manually set it to a specific place.

The list file for `tsmerge` may contain relative or absolute paths, with one path per line.
Alternatively, the list file may also be an M3U8 playlist file.

The `Quality` keywords available for the `videodownload` mode are:

- best, source, chunked
- 1080, 1080p, 1080p60, 1920x1080, 1920x1080p, 1920x1080p60
- 900, 900p, 900p60, 1600x900, 1600x900p, 1600x900p60
- 720, 720p, 720p60, 1280x720, 1280x720p, 1280x720p60
- 480, 480p, 480p60, 640x480, 640x480p, 640x480p60
- 360, 360p, 360p60, 480x360, 480x360p, 480x360p60
- 160, 160p, 160p60, 284x160, 284x160p, 284x160p60
- 144, 144p, 144p60. 256x144, 256x144p, 256x144p60
- worst
- audio

The framerate is detected based on the resolution, which is prioritized. If the framerate (like 30, 50, 60, 120 or any other) is manually set, will look for a stream that matches the provided parameters. If that stream does not exist, will default to best.
The audio stream is also the same audio stream for all video tracks. Twitch does not allow different audio tracks with different qualities for the same VOD ID. But there are different audio qualities to choose from before starting the stream.

#### Trimming and chapters

The mode `videodownload` has the options `-b` and `-e` to trim the output file at the beginning and/or at the end. The values are in whole seconds and relative to the full ID duration. The mode `clipdownload` doesn't have these options.

This trim splits the compressed video outside of I-frames (in copy mode) most of the time, and doesn't recode the affected GOPs, for the sake of speed and simplicity. Twitch streams don't always have I-frames placed at whole second marks (can be like 2.9899 seconds). Sometimes the GOPs can be spaced more than 10 seconds from each other, but usually it's 1.5-2.

As a result of this, the GOP at the beginning and/or the end will be split, leaving that part of it unplayable, until the next I-frame (Intra-Frame or key-Frame) is reached. The audio is not affected, either if it is alone or with video.

This is because Twitch streams use the H.264 (AVC) codec, which is a consumer-only video format designed for efficiency.

So when it's not cut by I-frame, although the audio and the video have the same duration, the playable video is less than the audio, and starts to play after the audio has been already reproducing for a while. Depending on the video player and its version:

- If the output file is only .m4a there's no issue, plays from beginning to end.
- Some video players play from the very start of the whole file, if at least one of the tracks is playable. They put a black frame on screen while the audio plays until everything is back in sync. `Quick Look` (Mac Spacebar Preview) does this. This is the most conversative approach and it's when people realize their video is broken.
- Other video players start playing from the timestamp when all tracks can be decoded properly, so they wait until the video can be played, and skip that portion of audio. Some people may never know that there's more audio inside. Forcing to play audio only, like `mpv --no-video output.mp4`, forces to play the entire audio stream.

To avoid losing video content, trim the beginning earlier, and the end later, so all the desired content falls into full GOPs. Chapters are not adjusted relative to trimmed file, `ffmpeg` can't do that automatically.
