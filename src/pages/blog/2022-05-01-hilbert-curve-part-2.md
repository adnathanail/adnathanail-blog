---
templateKey: blog-post
title: "Hilbert curve: part 2"
date: 2022-05-01T16:43:40.247Z
description: Turning the Hilbert curve into a snake
featuredpost: true
featuredimage: /img/screenshot-2022-05-01-at-18.02.20.png
---
My original concept for this Hilbert curve experiment was to have a point move along a Hilbert curve, with different rows and columns representing different note values. It would maybe have 12 columns, for the 12 notes, or 8 for a given scale, and I was going to see what kind of music a Hilbert curve could generate on its own.

I may still do that, but I realised that what I'd made reminded me a lot of the classic game "snake", and a new idea came to me. The curve would essentially serve as an interesting path for a snake to follow, and then every time it ate a piece of food the note would change. I can easily position the food at correct spacings on the path, so that it can actually play music.

This was going to make the code a lot larger, so I decided to start splitting up my code, into classes to represent the snake and its food, and somewhere to stick all the utility functions.

The `GameObject` class is the base for both `Snake` and `SnakeFood`, and just holds a `hindex` (an integer representing a position along a Hilbert curve, counted from the first point in the top left) and functions that can turn that into coordinates.

```java
class GameObject { 
    int hindex;

    GameObject (int in_hindex) {
        this.hindex = in_hindex;
    }

    int get_location(int coord) {
        return (points[this.hindex][coord] + PADDING) * SCALE_FACTOR;
    }
    int get_x_location() {
        return get_location(0);
    }
    int get_y_location() {
        return get_location(1);
    }
} 
```

The `Snake` class builds on this, adding a `length` attribute and a `draw()` method

```java
class Snake extends GameObject { 
    int length;

    Snake (int in_hindex) {
        super(in_hindex);
        this.length = 1;
    }

    void draw() {
        int end_point = min(hindex, (N*N) - 1);
        for (int i = end_point - this.length; i < end_point; i++) {
            line(
                cached_h2x(i, 0),
                cached_h2x(i, 1),
                cached_h2x(i+1, 0),
                cached_h2x(i+1, 1)
            );
        }
    }
} 
```

And `SnakeFood` brings in some of the music, taking a `note` (A-G#) and an `octave` (0-8), which are converted to a frequency using a handy helper function. There is also a boolean `hidden` which is used when there are no more food to be eaten, so I just hide the last one for aesthetics sake.

```java
class SnakeFood extends GameObject {
    float frequency;
    boolean hidden;

    SnakeFood (String note, int octave, int in_hindex) {
        super(in_hindex);
        this.frequency = getFrequency(note, octave);
        this.hidden = false;
    }

    void draw() {
        if (!hidden) {
            point(
                get_x_location(),
                get_y_location()
            );
        }
    }
}
```

The last class is `Note` which is really just used as an intermediary between me and `SnakeFood`, as it defines an easier interface to enter music in, allowing a `duration` as opposed to a `hindex`. Then in the `setup` we convert one to the other.

```java
class Note {
    String note;
    int octave;
    int duration;

    Note (String in_note, int in_octave, int in_duration) {
        this.note = in_note;
        this.octave = in_octave;
        this.duration = in_duration;
    }
}
```

The `last2bits` and `hindex2xy` functions remain relatively unchanged, but refactored into the new \`utils.pde\` file. There is also a helper for getting the cached results:

```java
int cached_h2x(int hindex, int coord) {
    return (points[hindex][coord] + PADDING) * SCALE_FACTOR;
}
```

Then there is a function to convert a note name and an octave to a frequency that the Processing `sound` library can play

```java
float getFrequency(String note, int octave) {
    float keyNumber = NOTES.indexOf(note);

    if (keyNumber < 3) {
        keyNumber = keyNumber + 12 + ((octave - 1) * 12) + 1; 
    } else {
        keyNumber = keyNumber + ((octave - 1) * 12) + 1; 
    }

    // Return frequency of note
    return 440 * pow(2, (keyNumber - 49) / 12);
};
```

Now that everything is nicely split up, the main code file is much neater, and easier to work with. You can find the full code listing here

<https://github.com/adnathanail/hilbert-curve/tree/master/hilbert_curve_2>

All that was left to do was encode a song in my `Note` format, for which I chose [this copy of In the Hall of the Mountain King](https://makingmusicfun.net/pdf/sheet_music/in-the-hall-of-the-mountain-king.pdf)

And now, what you've all been waiting for, a demo:

`youtube: https://www.youtube.com/shorts/ylsKWghaiQg`