---
templateKey: blog-post
title: "Hilbert curve: Part 3"
date: 2022-05-08T01:31:39.101Z
description: In which I tire of manually writing out songs
featuredpost: true
featuredimage: /img/screenshot-2022-05-08-at-02.36.51.png
---
"Let's read MIDI files," I said. "It'll be easy," I said. "This is the standard musical interchange format, and has been for decades, it must be well-defined and simple to parse," I said.

Firstly, MIDI files are not (as I had assumed) text files, they are binary files, so I am unable to parse them directly. Luckily Java has the `javax.sound.midi` library which can do some of the work for me. Using it wasn't instantly straightforward, but I found [this handy article on the processing forums](https://discourse.processing.org/t/parse-midi-files/14126/2).

I found [a MIDI copy of In The Hall Of The Mountain King](https://bitmidi.com/in-the-hall-of-the-mountain-king-mid) and tested the code on it.

![A terminal window with details of MIDI events echoed out](/img/screenshot-2022-05-08-at-02.36.51.png)

This was a decent start as, with minimal effort from me, I had a list of notes taken from a MIDI file, which could easily be swapped out for basically any song in existence.

> One small note, for some reason the MIDI file I had had a random empty track on it.
>
> A **track** seems to be a bit of a modern invention on top of the original system of **channels**. A MIDI file/device/instrument can (normally) only have 16 output channels, as a holdover from when data was being sent down physical 16-wite cables. The ability to subdivide things further (e.g. each different drum in a kit having a separate track, but the same channel) can be useful during the editing process.
>
> My understanding is that I should be playing all tracks and all channels simultaneously.

The next thing to do was to try actually playing this, which means I need to convert MIDI `note` values to frequencies. I found a [handy gist on GitHub](https://gist.github.com/stuartmemo/3766449) which I was ready to convert to Java, but I typed out the function name `midiNoteToFrequency` and GitHub Copilot (which I forgot I had really) did it for me!

![A VS Code window with GitHub Copilot autocompleting my function](/img/img_6506.jpg "How cool is that!")

Ignoring channels, for now, I removed the `SONG` array and added a line to the loop in the MIDI demo code which would create `SnakeFood` objects, ready to be played. And here is the finished result!

`youtube: https://youtube.com/shorts/swHsJWYzkls?feature=share`

Hmm, something isn't quite right! From the screenshot I can see there are 2 channels (0 and 1) and I am just playing them both at the same time. Here are the two channels played separately

**Channel 0**

`youtube: https://www.youtube.com/shorts/Wa9UJ38dY3A`

**Channel 1**

`youtube: https://www.youtube.com/shorts/nyTa69RUBgI`

Channel 1 sounds a bit odd/boring but it's only a baseline, I'm sure it'll sound fine in context. Channel 0 is clearly recognizable and highlights my next problem: note duration. The proper song (as demonstrated in my last post) should have 6 quavers and then a crotchet, however here all notes are being played with the same duration so it just sounds like 7 quavers

I'll figure this out in part 4!