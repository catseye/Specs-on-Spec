Irishsea
========

Irishsea is an experiment in "live coding" or "cyber-physical programming"
(or maybe "cyberphysical livecoding", why not?)

It is vapourware.  I haven't even been as far as decided for what I want to
implement, or what language/environment to implement it in.  It is mostly,
for the time being, a collection of thoughts on the subject.  And they're
not even very coherent thoughts!  Don't try to make sense of them!

Let's back up.

Many of the ideas behind "cyber-physical programming" do not seem to be
actually very new.  Consider...

*   [Sketchpad][]
*   Front-panel lights on the [Altair 8800][]
*   Interactive debugging capabilities of [LISP machines][]
*   Turtle Logo
*   Smalltalk
*   Even most 8-bit BASICs let you interrupt a running program, change
    some variables, then issue a `CONT` to continue execution
*   "Expression Watch" windows in various IDE's
*   etc.

The main new idea seems to be:

*   _the effects of the operator's interactive reprogramming of the
    system are thought of as a kind of **performance**_.

This may be a literal performance, in the case of, say, a musical livecoding
concert.  Or, it may be something more informal, or more abstract, or of
supposedly practical value; but it is still some kind of... I don't know,
_experience_, for lack of a better word; potentially a shared experience.

Goals
-----

One of the goals of the Irishsea project is to come up with answers to
the question: _how do you play a computer like you play a musical instrument?_

Actually, I should say "performance instrument".  I said "musical instrument"
only because (a) you probably know what musical instruments look like, and
how they generally work, and (b) you probably don't have a good idea of what
a "performance instrument" would look like or how it would work.  (I know I
don't.)

Or, another way to arrange the confusion in the above two paragraphs:
Irishsea is a performance instrument, made out of a computer, using the
concept of a musical performance to *frame*, but not *limit*, what we mean
by "performance".

Working towards these goals might include:

*   define a model for programs that can "do performance" and/or
    whose executions *are* performances
*   define a language (protocol) for reprogramming the model
*   define an environment (UI) in which that language can be "spoken"

(Why do I keep saying "reprogramming?"  Because you almost never program a
computer from scratch.  Other people have already programmed it a lot before
you got your hands on it -- they built the OS, the text editors, the compilers
and interpreters that you use...  You can think of yourself just "programming"
it, because you are adding new code to the existing code, but you still have
to admit that your code does not live in a vacuum.  Unless maybe you like to
hand-assemble your own operating systems.)

### Ideas for Model ###

*   process- and messaging-based (like e.g. Erlang)
*   input/output devices look like processes and send/receive messages
*   see `doc/model.markdown` for more info

### Ideas for Language ###

*   also process- and messaging-based
*   terse, very terse, because you are to play it like an instrument
*   see `doc/language.markdown` for more info

### Ideas for Environment ###

*   visibility into what all the process are doing, and what they will be doing
     (insofar as that can be predicted)
*   see `doc/environment.markdown` for more info

Motivation
----------

Having done all of the following things:

* performed music
* composed music
* written software
* used software

I have a hard time reconciling musical performance with writing software.
They're very different activities, for me.  But I have less of a problem
reconciling

* performing music with composing it (they call this _improvisation_)
* composing music with writing software (they're not dissimilar)
* writing software with using it (you can call this _bootstrapping_)

So it seems theoretically possible.  So I'd like to try.  So this is me
trying.

Links
-----

*   [extempore](https://github.com/digego/extempore)
*   [circa](https://github.com/paulhodge/circa)
*   [vivace](https://github.com/automata/vivace)
*   [live unit tests demo](http://livecoding.staticloud.com/)

[Altair 8800]: http://en.wikipedia.org/wiki/Altair_8800
[Lisp machines]: http://en.wikipedia.org/wiki/Lisp_machine
[Sketchpad]: http://en.wikipedia.org/wiki/Sketchpad
