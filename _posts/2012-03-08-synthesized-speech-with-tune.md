---
layout: post
title: "Some fun with Apple's TUNE format"
description: "An introduction to Apple's TUNE format for customizing synthesized speech (and perhaps song?)"
category:
tags: []
---
{% include JB/setup %}

So I discovered this recently, and this is a great way to get some vocal samples from Apple's built-in voice synthesizer.

You may be familiar with the OSX command

    $ say "some text"
    
which causes whatever you typed in to come spewing back at you in pristine, robotic-sounding aural goodness.

If you look at the man page for it (```man say```), you can see it supports a host of other options like input/output files, voice settings, and some interesting network options.

What it doesn't show you, however, is a wonderful little file format called TUNE that Apple developed to use with their TTS (text-to-speech) system.

A TUNE file looks like this:

    [[inpt TUNE]]

      UW {D 2000; P 277.183:0 311.127:15 311.127:50 277.183:51 277.183:99}

    [[inpt TEXT]]

      Here's some text thats gonna come out of your computer's speakers.

lets break it down:

The 

    [[inpt TUNE]]

line specifies that the following input is in TUNE format. easy enough.

The

    [[inpt TEXT]]

line specifies that the following input is plain text, to be spoken 'normally'. Also pretty straightforward.

The interesting stuff, however, is in this line:

    UW {D 2000; P 277.183:0 311.127:15 311.127:50 277.183:51 277.183:99}

The first part, "UW", specifies the [phoneme](http://en.wikipedia.org/wiki/Phoneme) that is spoken by the synthesizer.

The rest of the line is duration and pitch control. Its a little confusing at first, so bear with me.

The first part between the brackets,

    D 2000;

specifies the total length of the phoneme.

The following parts,

    P 277.183:0 311.127:15 311.127:50 277.183:51 277.183:99

are all pitch control, with the syntax 

    <pitch-in-hz>:<percent-time>


Iterated for each pitch-change in the phoneme's utterance.
So in this case, this will start at 277 Hz, then go to 311 Hz at 15% of the total time (or 300ms), go to 277 Hz at 1020ms, and stay there for the rest of the sample.

One caveat though, is that the synthesizer will attempt to add its own slew to the sample, meaning it will move the pitch continuously the whole time, unless you specify the lengths of the pitches in more detail.

To get a sense of what I mean, take out the section ```311.127:50``` from the line. So you should end up with this:

    [[inpt TUNE]]

      UW {D 2000; P 277.183:0 311.127:15 277.183:51 277.183:99}<br />

    [[inpt TEXT]]

You will notice that between 300 ms and 1020 ms, the pitch will start at 311 Hz, and move continuously down to 277 Hz the whole time. If this is not desired, then you have to tell the program to stay at that 311 Hz until 50% of the time is up, then quickly switch to 277 Hz by the 51% mark, which is accomplished by adding

    311.127:50

to the line.

Also remember, the numbers after the colons should be moving in ascending order - so an order like

    0,15,50,51,99

like in the example is ok, but

    42,1,35,99 

wont work.

---

Here's a multi-phoneme example for you. I messed around a little after listening to [this song](http://www.youtube.com/watch?v=Q5BiRNfFBVI) to see what sounds I could come up with. ADD took over and I only ended up doing two - the first example and this one, but here's what I have so far:

    [[inpt TUNE]]
    ~     
    w { D 100; P 155.563:0 277.183:99}
    2EH { D 100; P 277.183:0 277.183:99}
    n { D 100; P 277.183:0 277.183:99}
    ~ { D 100; P 277.183:0 277.183:99}
    w { D 100; P 246.9421:0 246.9421:99}
    IY { D 100; P 246.9421:0 246.9421:99}
    _ { D 100; P 277.183:0 277.183:99}
    s { D 100; P 277.183:0 277.183:99}
    t { D 100; P 277.183:0 277.183:99}
    1AA { D 100; P 277.183:0 277.183:99}
    r { D 100; P 277.183:0 277.183:99}
    t { D 100; P 277.183:0 277.183:99}
    _ { D 100; P 277.183:0 277.183:99}
    m { D 100; P 277.183:0 277.183:99}
    1EY { D 100; P 277.183:0 277.183:99}
    k { D 100; P 277.183:0 277.183:99}
    =IH { D 100; P 277.183:0 277.183:99}
    N { D 100; P 246.183:0 246.183:99}
    _ { D 100; P 277.183:0 277.183:99}
    f { D 100; P 277.183:0 277.183:99}
    1AY { D 100; P 277.183:0 277.183:99}
    AX { D 100; P 277.183:0 277.183:99}
    r { D 200; P 311.127:0 246.183:25 246.183:99}
    z { D 100; P 207.652:0 207.652:99}
    . 
    [[inpt TEXT]]

If you want more reference to this, you can check out Apples's documentation on the speech synthesizer and TUNE [here](http://developer.apple.com/library/mac/#documentation/UserExperience/Conceptual/SpeechSynthesisProgrammingGuide/SpeechOverview/SpeechOverview.html).
So go ahead and mess around with that, and shoot me an email or sample if you happen to make anything interesting!
