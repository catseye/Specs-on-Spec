The Didigm Reflective Cellular Automaton
========================================

November 2007, Chris Pressey, Cat's Eye Technologies

Introduction
------------

Didigm is a *reflective cellular automaton*. What I mean to impart by
this phrase is that it is a cellular automaton where the transition
rules are given by the very patterns of cells that exist in the
playfield at any given time.

Perhaps another way to think of Didigm is: Didigm = [ALPACA][] + [Ypsilax][].

[ALPACA]: http://catseye.tc/node/ALPACA.html
[Ypsilax]: http://catseye.tc/node/Ypsilax.html

Didigm as Parameterized Language
--------------------------------

Didigm is actually a parameterized language. A parameterized language is
a schema for specifying a set of languages, where a specific language
can be obtained by supplying one or more parameters. For example,
[Xigxag][] is a parameterized language, where the
direction of the scanning and the direction of the building of new
states are parameters. Didigm "takes" a single parameter, an integer.
This parameter determines how many *colours* (number of possible states
for any given cell) the cellular automaton has. This parameter defaults
to 8. So when we say Didigm, we actually mean Didigm(8), but there are
an infinite number of other possible languages, such as Didigm(5) and
Didigm(70521).

[Xigxag]: http://catseye.tc/node/Xigxag.html

The languages Didigm(0), Didigm(-1), Didigm(-2) and so forth are
probably nonsensical abberations, but I'll leave that question for the
philosophers to ponder. Didigm(1) is at least well-defined, but it's
trivial. Didigm(2) and Didigm(3) are semantically problematic for more
technically interesting reasons (Didigm(3) might be CA-universal, but
Didigm(2) probably isn't.) Didigm(4) and above are easily shown to be
CA-universal.

(I say CA-universal, and not Turing-complete, because technically
cellular automata cannot simulate Turing machines without some extra
machinery: TMs can halt, but CAs can't. Since I don't want to deal with
defining that extra machinery in Didigm, it's simpler to avoid it for
now.)

Colours are typically numbered. However, this is not meant to imply an
ordering between colours. The eight colours of Didigm are typically
referred to as 0 to 7.

Language Description
--------------------

### Playfield

The Didigm playfield, called *le monde*, is considered unbounded, like
most cellular automaton playfields, but there is one significant
difference. There is a horizontal division in this playfield, splitting
it into regions called *le ciel*, on top, and *la terre*, below. This
division is distinguishable — meaning, it must be possible to tell which
region a given cell is in — but it need not have a presence beyond that.
Specifically, this division lies on the edges between cells, rather than
in the cells themselves. It has no "substance" and need not be visible
to the user. (The Didigm Input Format, below, describes how it may be
specified in textual input files.)

### Magic Colours

Each region of the division has a distinguished colour which is called
the *magic colour* of that region. The magic colour of le ciel is colour
0. The magic colour of la terre is colour 7. (In Didigm(n), the magic
colour of la terre is colour n-1.)

### Transition Rules

#### Definition

Each transition rule of the cellular automaton is not fixed, rather, it
is given by certain forms that are present in the playfield.

Such a form is called *une salle* and has the following configuration.
Two horizontally-adjacent cells of the magic colour abut a cell of the
*destination colour* to the right. Two cells below the rightmost
magic-colour cell is the cell of the *source colour*; it is surrounded
by cells of any colour called the *determiners*.

This is perhaps better illustrated than explained. In the following
diagram, the magic colour is 0 (this salle is in le ciel,) the source
colour is 1, the destination colour is 2, and the determiners are
indicated by D's.

    002
    DDD
    D1D
    DDD

#### Application

Salles are interpreted as transition rules as follows. When the colour
of a given cell is the same as the source colour of some salle, and when
the colours of all the cells surrounding that cell are the exact same
colours (in the exact same pattern) as the determininers of that salle,
we say that that salle *matches* that cell. When any cell is matched by
some salle in the other region, we say that that salle *applies* to that
cell, and that cell is replaced by a cell of the destination colour of
that salle.

"The other region" refers, of course, to the region that is not the
region in which the cell being transformed resides. Salles in la terre
only apply to cells in le ciel and vice-versa. This complementarity
serves to limit the amount of chaos: if there was some salle that
applied to *all* cells, it would apply directly to the cells that made
up that salle, and that salle would be immediately transformed.

On each "tick" of the cellular automaton, all cells are checked to find
the salle that applies to them, and then all are transformed,
simultaneously, resulting in the next configuration of le monde.

There is a "default" transition rule which also serves to limit the
amount of chaos: if no salle applies to a cell, the colour of that cell
does not change.

Salles may overlap. However, no salle may straddle the horizon. (That
is, each salle must be either completely in le ciel or completely in la
terre.)

Salles may conflict (i.e. two salles may have the same source colour and
determiners, but different destination colours.) The behaviour in this
case is defined to be uniformly random: if there are n conflicting
salles, each has a 1/n chance of being the one that applies.

Didigm Input Format
-------------------

I'd like to give some examples, but first I need a format to given them
in.

A Didigm Input File is a text file. The textual digit symbols `0`
through `9` indicate cells of colours 0 through 9. Further colours may
be indicated by enclosing a decimal digit string in square brackets, for
example `[123]`. This digit string may contain leading zeros, in order
for columns to line up nicely in the file.

A line containing only a `,` symbol in the leftmost column indicates the
division between le ciel and la terre. This line does not become part of
the playfield.

A line beginning with a `=` is a directive of some sort.

A line beginning with `=C` followed by a colour indicator indicates how
many colours (the n in Didigm(n)) this playfield contains. This
directive may only occur once.

A line beginning with `=F` followed by a colour indicator as described
above, indicates that the unspecified (and unbounded) remainder of le
ciel or la terre (whichever side of `,` the directive is on) is to be
considered filled with cells of the given colour.

Of course, an application which implements Didigm with some alternate
means of specifying le monde, for example a graphical user interface,
need not understand the Didigm Input Format.

Examples
--------

Didigm is immediately seen to be CA-universal, in that you can readily
(and stably) express a number of known CA-universal cellular automata in
it. For example, to express John Conway's Life, you could say that
colour 1 means "alive" and colour 2 means "dead", and compose something
like

    002002001001
    222122112212 ... and so on ...
    212212212121 ... for all 256 ...
    222222222222 ... rules of Life ...
    =F3
    ,
    =F2
    22222
    21222
    21212
    21122
    22222

Because the magic colour 7 never appears in la terre, il n'y a aucune
salle dans la terre et donc tout le ciel est toujours la meme chose.

There are of course simpler CA's that are apparently CA-universal that
would be possible to describe more compactly. But more interesting (to
me) is the possibility for making reflective CA's.

To do this in an uncontrolled fashion is easy. We just stick some salles
in le ciel, some salles in la terre, and let 'er rip. Unfortunately, in
general, les salles in each region will probably quickly damage enough
of the salles in the other region that le monde will become sterile soon
enough.

A rudimentary example of something a little more orchestrated follows.

    3333333333333
    3002300230073
    3111311132113
    3311321131573
    3111311131333
    3333333333333
    =F3
    ,
    =F1
    111111111111111
    111111131111111
    111111111111574
    111111111111333
    311111111111023
    111111111111113

The intent of this is that the 3's in la terre initially grow streams of
2's to their right, due to the leftmost two salles in le ciel. However,
when the top stream of 2's reaches the cell just above and to the left
of the 5, the third salle in le ciel matches and turns the 5 into a 7,
forming une salle dans la terre. This salle turns every 2 to the right
of a 0 in le ciel into a 4, thus modifying two of les salles in le ciel.
The result of these modified salles is to turn the bottom stream of 2's
into a stream of 4's halfway along.

This is at least predictable, but it still becomes uninteresting fairly
quickly. Also note that it's not just the isolated 3's in la terre that
grow streams of 2's to the right: the 3's on the right side of la salle
would, too. This could be rectified by having a wall of some other
colour on that side of la salle, and I'm sure you could extend this
example by having something else happen when the stream of 4's hit the 0
in that salle, but you get the picture. Creating a neat and tidy and
long-lived reflective cellular automaton requires at least as much care
as constructing a "normal" cellular automaton, and probably in general
more.

History
-------

I came up with the concept of a reflective cellular automaton (which is,
as far as I'm aware, a novel concept) independently on November 1st,
2007, while walking on Felix Avenue, in Windsor, Ontario.

No reference implementation exists yet. Therefore, all Didigm runs have
been thought experiments, and it's entirely possible that I've missed
something in its definition that having a working simulator would
reveal.

Happy magic colouring!  
Chris Pressey  
Chicago, Illinois  
November 17, 2007
