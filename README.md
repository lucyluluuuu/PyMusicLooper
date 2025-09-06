## Pre-requisites

# The following software must be installed for `pymusiclooper` to function correctly.

- [Python (64-bit)](https://www.python.org/downloads/) >=3.10
- [ffmpeg](https://ffmpeg.org/download.html):
- [yt-dlp](https://github.com/yt-dlp/yt-dlp)) required for loading audio from youtube (or any stream supported by [yt-dlp](https://github.com/yt-dlp/yt-dlp)) and adds support for loading additional audio formats and codecs such as M4A/AAC, Apple Lossless (ALAC), WMA, ATRAC (.at9), etc. A full list can be found at [ffmpeg's documentation](https://www.ffmpeg.org/general.html#Audio-Codecs). If the aforementioned features are not required, can be skipped.

## Installation

- First install Chocolatey with this powershell command 
- Make sure your powershell is elevated aka Run as administrator 
```sh
Set-ExecutionPolicy Bypass -Scope Process -Force; [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; iex ((New-Object System.Net.WebClient).DownloadString('https://community.chocolatey.org/install.ps1'))
```
- Then run this long commmand all at once in powershell
- Make sure your powershell is elevated aka Run as administrator 
- Run these commands from wherever you want to install pymusiclooper 
- i.e. run cd C:\Users\"YourUsernameHere"\Documents\pymusiclooper in powershell BEFORE running the command below 
```sh
choco install ffmpeg-full python git -y ; refreshenv ; pip install yt-dlp --upgrade build hatchling ; git clone https://github.com/lucyluluuuu/PyMusicLooper.git ; cd PyMusicLooper ; python -m build ; pip install --force-reinstall dist/pymusiclooper-*.whl
```
- Install K-Lite Codec Pack. It's probably fine without it but having codecs up to date never hurts </pre>
- Run the curl.exe command in an elevated powershell </pre>
```sh
curl.exe -L "https://files2.codecguide.com/beta/K-Lite_Codec_Pack_1918_Full.exe" -o "$env:TEMP\klite.exe"
Start-Process "$env:TEMP\klite.exe" -ArgumentList "/verysilent" -Wait
```
- Run refreshenv and then you should be able to run pymusiclooper -

## Available Commands

![pymusiclooper --help](https://github.com/arkrow/PyMusicLooper/raw/master/img/pymusiclooper.svg)

 further help and options can be found in each subcommand's help message (e.g. `pymusiclooper export-points --help`); </pre>
 all commands and their `--help` message can be seen in [CLI_README.md](https://github.com/arkrow/PyMusicLooper/blob/master/CLI_README.md) </pre>

 Using the interactive `-i` option is highly recommended, since the automatically chosen "best" loop point may not necessarily be the best one perceptually. </pre>
 As such, it is shown in all the examples. Can be disabled if the `-i` flag is omitted. Interactive mode is also available when batch processing. </pre>

## Example Usage

### Play

```sh
# Play the song on repeat with the best discovered loop point.
pymusiclooper -i play --path "TRACK_NAME.mp3"


# Audio can also be loaded from any stream supported by yt-dlp, e.g. youtube
# (also available for the `tag` and `split-audio` subcommands)
pymusiclooper -i play --url "https://www.youtube.com/watch?v=dQw4w9WgXcQ"


# Reads the loop metadata tags from an audio file and play it with the loop active
# using the loop start and end specified in the file (must be stored as samples)
pymusiclooper play-tagged --path "TRACK_NAME.mp3" --tag-names LOOP_START LOOP_END
```

### Export

*Note: batch processing is available for all export subcommands. Simply specify a directory instead of a file as the path to be used.*

```sh
# Split the audio track into intro, loop and outro files.
pymusiclooper -i split-audio --path "TRACK_NAME.ogg"

# Extend a track to an hour long (--extended-length accepts a number in seconds)
pymusiclooper -i extend --path "TRACK_NAME.ogg" --extended-length 3600

# Extend a track to an hour long, with its outro and in OGG format
pymusiclooper -i extend --path "TRACK_NAME.ogg" --extended-length 3600 --disable-fade-out --format "OGG"

# Export the best/chosen loop points directly to the terminal as sample points or time in mm:ss.sss or time in   # seconds only
pymusiclooper -i export-points --path "/path/to/track.wav" --fmt samples/time/seconds
                                                                   ^ (if not specified defaults to samples) 

# Export all the discovered loop points directly to the terminal as sample points
# Same output as interactive mode with loop values in samples, but without the formatting and pagination
# Format: loop_start loop_end note_difference loudness_difference score
pymusiclooper export-points --path "/path/to/track.wav" --alt-export-top -1

# Add metadata tags of the best discovered loop points to a copy of the input audio file
# (or all audio files in a directory, if a directory path is used instead)
pymusiclooper -i tag --path "TRACK_NAME.mp3" --tag-names LOOP_START LOOP_END


# Export the loop points (in samples) of all tracks in a particular directory to a loops.txt file
# (compatible with https://github.com/libertyernie/LoopingAudioConverter/)
# Note: each line in loop.txt follows the following format: {loop-start} {loop-end} {filename}
pymusiclooper -i export-points --path "/path/to/dir/" --export-to txt
```

## Credit

This is a fork of https://github.com/arkrow/PyMusicLooper by arkrow
