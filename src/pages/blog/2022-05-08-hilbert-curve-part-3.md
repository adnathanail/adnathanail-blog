---
templateKey: blog-post
title: "Hilbert curve: part 3"
date: 2022-05-08T01:31:39.101Z
description: In which I tire of manually writing out songs
featuredpost: true
featuredimage: /img/screenshot-2022-05-08-at-02.36.51.png
---
> "Lets read MIDI files" I said. "It'll be easy" I said. "This is the standard musical interchange format, and has been for decades, it must be well-defined and simple to parse" I said.

Firstly, MIDI files are not (as I had assumed) text files, they are binary files, so I am unable to parse them directly. Luckily Java has the `javax.sound.midi` library which can do some of the work for me. Using it wasn't instantly straightforward, but I found [this handy article on the processing forums](https://discourse.processing.org/t/parse-midi-files/14126/2).

I found [a MIDI copy of In The Hall Of The Mountain King](https://bitmidi.com/in-the-hall-of-the-mountain-king-mid) and tested the code on it.

![A terminal window with details of MIDI events echoed out](/img/screenshot-2022-05-08-at-02.36.51.png)

This was a decent start as, with minimal effort from me, I had a list of notes taken from a MIDI file, which could easily be swapped out for basically any song in existance.

> One small note, for some reason the MIDI file I had had a random empty track on it.
>
> A **track** seems to be a bit of a modern invention on top of the original system of **channels**. A MIDI file/device/instrument can (normally) only have 16 output channels, as a holdover from when data was being sent down physical 16-wite cables. The ability to subdivide things further (e.g. each different drum in a kit having a separate track, but the same channel) can be useful during the editing process.
>
> My understanding is that I should be playing all tracks and all channels simultaneously.