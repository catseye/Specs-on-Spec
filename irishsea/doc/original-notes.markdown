Thoughts about livecoding and related activities
================================================

This is a random collection of notes (not necessarily particularly
intelligent ones, either) comparing some technical and creative activities.

*   Here are a few of the technical and creative activities I've undertaken:
    
    * Computer programming
    * Programming language design
    * Musical composition
    * Musical performance

*   These can be combined.
    
    Doing musical composition during a musical performance can be called
    _musical improvisation_.  I've done this.
    
    Doing computer programming as a means of musical improvisation can be
    called _musical livecoding_.  I've never done this.

*   Livecoding, eh?
    
    I only recently encountered livecoding.  These notes are largely the
    result of me trying to come to grips with the concept.  I'm coming at
    it in a very raw way;  I basically stumbled upon it on Github.  I've
    never experienced a livecoding performance.  But I think that inexperience
    might let me think about it in a less biased way, too.

*   I have a hard time reconciling computer programming with musical improvisation.
    
    My experience with computer programming is similar to my experience with musical
    composition.
    
    I know I am able to musically improvise.  So it must be theoretically possible to
    reconcile the two.  But there are several things to account for.  Here are a few.

*   Engineering is not an eager algorithm.  But improvisation needs to be be.
    
    Any software project of any non-trifling size requires thought and planning and
    structure and being broken up into components and interfaces and invariants and
    ideally has a test suite, too.
    
    This can't be done effectively with an eager algorithm — the closest you can
    come to that is probably a quick prototype followed by a series of incremental
    improvements and refactorings.  But even then, if you're not willing to do an
    occasional rewrite (i.e. significant rethink/refactoring of the design/code) at
    some point, you can paint yourself into a corner.  And the rewrites require
    thought and planning and consideration and all that stuff that takes some time,
    i.e. not an eager algorithm.
    
    But improvisation, for the most part, requires that you compose something
    "on the fly" — you don't have time to sit down and plan it.  But, in practice,
    that is not quite true; jazz musicians *practice* improvisation, and one of the
    results is they build up a stock of experience about improvisation that they can
    draw on, "on the fly".  Some of this is accumulating a library of "licks", but 
    much of it is also about building an intuition of what "works" with what.
    
    Also — when I played jazz, and had to play a solo for actual performance, I
    tended to refine a particular improvisation during practice, and play that
    during performance.  But that's just an instance of composition.  (I see nothing
    wrong with this approach, but some other musicians might feel that a solo
    played this way isn't "from the heart", or something.  It might be interesting
    to know just how much of their solo your average musician works out beforehand,
    and how much they actually make up during performance.  I'm sure it varies.)

*   Music is a performance art.  Programming isn't.
    
    Generally, no one is watching you program.  (Pair programming excepted, I
    suppose.)

*   Musical performance is "write-only".  Composition isn't.  Programming isn't.
    
    In a live performance, as soon as you hit a piano key, or as soon as you blow
    into the mouthpiece of your horn, the note you played is committed to the
    sound waves in the air.  There's no going back.  If you cacked it, if it was
    the wrong note, it was the wrong note and *it already happened*.  Sorry!
    
    Putting together a composition on with a sequencer (which is how the vast
    majority of synthesized music is done) is not like that at all.  You can
    arrange a few events, try it out, erase it, modify it until it's how you want
    it.
    
    Livecoding seems to necessarily fall under the first category.  Even though
    you can perhaps backspace to correct a line of code before it causes anything
    to happen, as soon as you do commit it, it's committed, and it's going
    to start having an affect on the performance, as soon as those events are
    queued up to go.
    
    (I suppose a livecoding language could allow you to cancel changes you
    recently made, if the events they queued up haven't happened yet.)

*   I have ten fingers, two hands, two arms, two feet, and a mouth and lungs.
    
    Almost every instrument I can think of uses some combination of those to
    control it.  Some examples:
    
    * piano: 10 fingers, 2 feet
    * drumkit: 2 hands, 2 feet
    * tuba: 4 fingers, mouth, lungs
    * trombone: 1 hand/arm, mouth, lungs
    * penny whistle: 8 fingers, lungs
    
    etc.
    
    A computer keyboard would be classified: 10 fingers.
    
    In combination with the idea that musical performance is "write-only", though:
    typically those 10 fingers must produce some combination of symbols, which
    is then comitted only after it is complete.  Also, it is generally assumed
    there is *visual feedback* involved in letting the operator compose such a
    sequence (i.e. I need to see the words I am typing, or I won't be able to
    type anything without making a typo.  I won't be able to know what I typed.)
    
    This all means the computer keyboard, as a musical instrument interface, is
    somewhat similar to, but also significantly different from, all other musical
    instruments' interfaces.

*   Livecoding is about external events.  With programs, it varies.
    
    I'm assuming that the livecoding we're dealing with here (so far) is a
    performance art technique in that the events that are caused by the coding
    have an observable effect outside of the computer.  They set up sounds to
    be played as part of music, lights, video, animation, dance, etc. etc.
    
    Programs, on the other hand, don't have to be about external events.  In
    practice, they are: even a simple program produces output when it's finished,
    and that's an external event.
    
    But programs (especially in esolang circles) can also be seen merely as
    _embodiments of computations_.  Many esolangs don't even have input and
    output — they just do a computation, and you examine the state of the
    virtual machine (or whatever) when it is finished executing, to see the
    result.
    
    Nevertheless, speaking *operationally*, the execution of a program can also
    be seen entirely as a sequence of events — assign a value to a variable,
    add two values, etc.  These are all events, albeit _internal_ events which
    are usually not detectable (unless you are debugging the program.)

*   "Pure" livecoding?
    
    Examining the previous point, if the internal events of an executing program
    are exposed and made observable to an audience... livecoding with that
    program would not be tied to some other media like music or dance.  This would
    be "pure" livecoding.
    
    It would obviously have a much smaller audience.
    
    But there are some interesting potentials here.  If I assign sounds to be
    played when certain things happen in the internal event model of my program,
    might that help me debug it?  Might certain algorithms produce inherently
    interesting patterns of events?  (cf bytebeat.  And things like John Conway's
    Game of Life, which is basically a computation, but often pleasing to watch.)

*   Computer is so much faster than me!  What chance do I stand!
    
    If a program is producing a series of events at a musical time scale, then it
    is not unlikely that I could write a line of code and execute it in time, before
    the next musical sequence comes up, so I can affect the song in real-time.
    
    If the events are at a computational time scale — that is SO much faster than
    even the musical time scale that there is no chance that I could, for example,
    modify the computation of a function before it is done.  (Only if it is operating
    on a very large amount of data, or is an inherently complex function, such as
    the Ackermann function, is it really even conceivable.)

*   Coding isn't necessarily general programming.
    
    "Coding" refers to writing a program in a Turing-complete programming language,
    but it can also refer to writing things in much weaker languages.  For example
    it is possible to say "coding an HTML page".  A purist might not want to call
    this "programming", maybe instead preferring "configuration".
    
    I will guess that most livecoding is configuration, not general programming.
    This is very reasonable, and a natural consequence of many of the things I've
    already mentioned.  General programs require engineering, which is slow, and
    are hard to debug in part because they're done in "powerful" languages.
    
    You really, really wouldn't want to have to debug a program during a live
    performance!
    
    At the same time, it would be really neat to have livecoding be about more
    than just reconfiguring event streams — it would be awesome to have some kind
    of "higher" computational aspect to it too.

What this all might mean for the design of a livecoding language/environment
============================================================================

*   The operator ought to be able to enter a change quickly.  But they also ought
    to be able to edit it easily before they commit it.
    
    Assuming we're sticking with the classical computer keyboard as the input
    device, this suggests to me that the most frequently used alterations should
    be performed with _short sequences of keystrokes which do not require multiple
    key combinations_ (so, no "shifted" symbols, like capital letters — of course,
    this varies internationally from one keyboard layout to another.)
    
*   Assuming there are event "generators" or "streams" which produce events that you
    want, until told to do otherwise, (for example, a drum track loop,) it seems
    like the most obvious operation is **tell a given generator to change what it
    will generate**.  If we think of the generators as processes (a la Erlang),
    this operation is just sending a message to a generator.
    
    If we are content with 26 generators, we can use lowercase letters to identify
    them.  Sending a message to a generator is a matter of naming the generator
    and then giving it the message you want to send to it.

*   Each generator should be pre-programmed (and perhaps re-programmable on the fly)
    to understand a certain set of messages.
    
    That is, each generator runs some "code" which reacts to events — a _handler_.
    Multiple generators could run the same handler.
    
    If a generator receives a message it doesn't understand, nothing should happen
    (well, maybe some feedback should be given to the operator, but other than that,
    nothing.)

*   So as an example, to send the message `p5` to the generator `a`, the operator
    might type:
    
        ap5↵
    
    (where ↵ is the Enter key)
    
    Presumably the handler that generator `a` is running knows what `p5` means.
    It might be a command that says "switch to pattern number five at the next
    transition."  In this case, `p` is the command, and `5` is a parameter to it.
    And assuming we have some rules for parsing commands and arguments like this
    (the simplest is that a space ends a command sequence) the operator could send
    several messages to one or more generators in one action, like
    
        ap5 bp7 br0↵
    
    (Maybe `r0` means "turn off reverb" or something.)  Or, if we have even more
    knowledge of the syntax, we might be able to shorten that to
    
        ap5bp7r0↵
    
    but this gets tricky, as we might want to have more complex arguments.  This
    syntax, obviously, needs formalizing.  But the point is to demonstrate that you
    could have such a syntax.  And you could type it quickly enough to do it during
    a performance — even a fast tempo performance, if you got practiced at it.

*   It might be useful for the editor to know about the syntax and do "structured
    editing", too; so that if, for example, `d` was not a valid message for the
    handler running in generator `a`, the editor would not even let you type `ad`.

*   Taking an idea from the design of the piano: maybe SHIFT and CTRL modifier keys
    could be foot pedals!  (Or is that too weird?)

*   Most changes take effect "at the next transition point".  (Computer is so much
    faster than me!  Music is too, for that matter!  But transitions should be
    kept smooth.)  There might be some exceptions.

*   Programming the generators to know what kind of commands to accept at runtime is
    a large part of this.  It's akin to accumulating those "licks" when practicing
    improvisation — teach the generator a set of tricks, and tell it what tricks to
    perform during the performance.  Each trick can be arbitrarily complicated (and
    you don't have to debug it during the performance!)  But the set of known tricks
    is limited.  *Unless* you have good ways to sensibly combine tricks into new
    tricks on the fly (without everything falling apart.)  This is where it seems
    very interesting.

*   Following that up, generators probably ought to be able to send messages to other
    generators.  This would let them act as proxies (send a message to one generator,
    it translates/filters it, and passes it on to one or more other generators) and
    other things, e.g. periodically send "increase amplitude" events to all other
    generators to achieve a global crescendo.

*   For that matter, the output devices can be thought of as processes which receive
    messages, too; so a generator for producing a percussion track would periodically
    be sending "bass" and "snare" messages to the "drum machine output device".  Would
    this use the same syntax as the operator's syntax for sending messages?  Ideally,
    yes, or at least something similar/compatible.

*   Operator ought to be able to start up a generator with a particular handler (i.e.
    the "code" that the generator is running, which knows what to do with particular
    events).  Switching handlers, and "tearing down" a generator could be handled by
    responding to messages, so the language probably doesn't need built-in operations
    for those.  (But maybe forcibly terminating a generator would be one.  And clearing
    a generator's message queue could be another — effectively, "cancel" what I've
    previously told this generator to do.)

*   This process/message model is very similar to Erlang's in many ways.

*   Multiple video monitors would probably be useful... but regardless, it would be useful
    for the operator to see a summary of **which generators are running what handlers** in
    real-time.
