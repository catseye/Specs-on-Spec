The Sartre Programming Language
===============================

John Colagioia, 199?

Introduction
------------

The Sartre programming language is named for the late existential
philosopher, Jean-Paul Sartre. Sartre is an extremely unstructured
language. Statements in Sartre have essentially no philosophical
purpose; they just are. Thus, Sartre programs are often left to define
their own functions.

Unlike traditional programming languages (or maybe very much like them),
nothing in Sartre is guaranteed, except maybe for the fact that nothing
is guaranteed. The Sartre compiler, therefore, must be case insensitive
(technically, it requires all capital letters, but since nothing matters
anyway, why should this?).

Names in Sartre may only contain letters (and only capital letters, at
that, but nobody really cares that much), and maybe some trailing
digits, but nothing else.

No standard mathematical functionality is supplied in Sartre. Instead,
nihilists are created, which may damage the properties inherent to the
data, and nihilators are executed to reclaim storage dynamically.

Sartre programmers, perhaps somewhat predictably, tend to be boring and
depressed, and are no fun at parties. No comments will be made on the
level of contrast between the Sartre programmer and any other
programmer.

In the words of a Sartre programmer who worked intensely for months,
eating whatever junkfood wandered near his cubicle, "I have been gaining
twenty-five pounds a week for two months, and I am now experiencing
light tides. It is stupid to be so fat. My pain, ultimate solitude, and
collection of Dilbert cartoons are still as authentic as they were when
I was thin, but seem to impress girls far less. From now on, I will live
on cigarettes and black coffee," which is the general diet of the Sartre
programmer, for obvious reasons.

Comments are available in Sartre, though not at all suggested, since
nobody really wants to listen to you, anyway, by typing the comment at
the beginning of the line, and terminating it (on the same line) with a
squiggle (brace) pair ("{}"). Valid Sartre text may be placed after the
comment if desired. If comments are absolutely necessary, they should
adequately describe the futility of the program and the plight of
programmer and computer in a world ruled by an unfeeling God and His
compilers, as well as providing explanation of the surrounding program
statements.

They should not be misspelled, as some compilers may check.

Admittedly, while it is not hard to string Sartre statements together to
create a Sartre-compilable text file, it can be quite hard to program in
the Sartre paradigm. To wit, one may keep creating programs, one after
another, like soldiers marching into the sea, but each one may seem
empty, hollow, like stone. One may want to create a program that
expresses the meaninglessness of existence, and instead they average two
numbers.

Sartre Data Types
-----------------

The Sartre language has two basic data types, the EN-SOI and the
POUR-SOI. The en-soi is a completely filled heap of a specified rank,
whereas the pour-soi is a dynamic structure which never has the same
value. An integer may also be used in Sartre, but it may only take the
value of zero (the Dada extensions to Sartre allow integers to also take
on the value of "duck sauce", but that's neither here nor there--unless
you happen to like duck sauce, of course).

en-soi

The en-soi, as mentioned before, is a full heap of a specified rank. As
Sartre does not allow pre-initialized data, the actual data in the heap
is non-specified (but the heap is "pre-heapified"). Data (en-sois of
rank 0) may be "deconstruct"ed from the en-soi, or "rotate"d through. At
all times, the en-soi remains a full heap, however. En-sois of rank zero
are 32 bits with no inherent meaning. En-sois of higher rank may be
defined as each element being of another (same) data type (an en-soi of
integers, all with a value of zero (or duck sauce), for example).

pour-soi

The pour-soi is only, and precisely, what it is not. It may be
"unassigned from" a certain value, thereby exactly increasing the number
of things it isn't. It is specified to be a two-bit value, so it
probably isn't.

integer

Unlike the integers in most programming languages, Sartre integers all
have a value of zero (again, unless the Dada extensions are being used).
Like the rest of the dreary universe (duck sauce included), this is
something that must be lived with.

orthograph

The orthograph is a special type of pictogram used in the specification
of lexicographic elements. The set of orthographs varies from Sartre
implementation to Sartre implementation, but is guaranteed to contain
all the so-called "letters" from at least one modern language,
transliterated to the closest element of the ASCII set and ordered as
the bit-reversed EBCDIC value, assuming those bit-patterns were
integers, which they probably aren't. Unavailable orthographs in a given
implementation are represented by "frowny faces". As defined, the
orthograph is possibly the simplest and most convenient data type to
work with.

const

Not an actual data type, but somewhat useful, is the introduction of the
symbolic constant into the Sartre language. Since Sartre does not allow
for unconventional, potentially confusing symbols to be strewn about a
program (for example, "17" meant to represent a certain quantity of
items), this allows the programmer to define a set of symbolic constants
he plans to use. To avoid confusion, symbolic constants are defined in
unary, using the "wow" (!) as the unit (i.e., !, !!, !!!, !!!!, ...).

Symbolic constants must be surrounded in "rabbit ears" (") in use during
the action section of the program.

Predefined Sartre Instances
---------------------------

The following data instances are provided to the Sartre programming
environment to facilitate programming certain concepts which would be
(and probably should be) nearly impossible otherwise.

                MAXINT  This is the maximum integer value allowed by the
                        particular Sartre implementation:  zero.
                MININT  This is the minimum integer value allowed by the
                        particular Sartre implementation.  If using the Dada
                        extensions, MININT is duck sauce; if not, it is zero.
                ORTH0   This is the "initial orthograph" of the Sartre
                        implementation.
                ORTHL8  This is the "final orthograph" of the implementation.
                        The name is properly pronounced "Orthograph:  Lazy
                        Eight".

Sartre Program Segments
-----------------------

The Sartre program is broken into simple, logical portions. In essence,
all things must be declared before usage, and the declaration section
comes before the action section (if any). Since the Sartre language has
a recursive structure, the Sartre nihilist has the same structure as the
main nihilator which is about to be described:

                    Nihilator <name> ;
                    {Nihilist;}
                    Const   <name> = <value>;
                    Consts  <name> = <value>..<value>;
                    Matter  {<name> {, <name>} : <type> ;}
                    Act
                            {<statement> ;}
                    No more ;
                    .

The only difference between a nihilist and the nihilator is that a
nihilist does not use the trailing one-spot.

Example Matter definitions (data declaration) might be:

                    Const   3 = !!;
                    Matter  dooM:   integer;
                            Rniqqlj:en-soi, rank "3", of pour-soi;

which creates an integer (with a value of 0, since we are not invoking
Dada extensions), and a heapified en-soi with seven pour-soi storage
locations.

Sartre Statement Types
----------------------

`IF ;`

The Sartre conditional takes no arguments and then alters program flow
accordingly. Put simply, on the condition where the most recently
executed nihilator was successful, program execution is transferred to
just beyond the next conditional, restarting the search from the
beginning, if necessary.

`LVAL := expr ;`

The Sartre assignment statement takes the bourgois-perceived value of
the damned expression and places it in LVAL. If LVAL is a pour-soi, this
unassigns a value from the pour-soi.

`No Exit ;`

A reminder to the program that none of us can escape what we have
wrought, or even escape what others have wrought. In programming terms,
this may either cause the machine to hang or cause the program not to
terminate, depending on the implementation.

`Life Is Meaningless ;`

A special command which, due to the resignation of the programmer, is
permitted to perform a wide variety of tasks, among them, alter the
direction of program flow, execute a random function, terminate the
program, or positionally invert the bits in the data region. Since the
programmer doesn't care anyway, this doesn't really matter. In the
(tee-hee) ordinary version of Sartre, this operation is defined at
compile-time, and is constant at that statement for each incidence of
execution. The Dada extensions, however, redefine meaninglessness (since
everything under Dada is meaningless to begin with) to be determined at
run- time. Further, it may also logically negate each bit in the
dataspace under Dada.

`{ data } <nihilist> ;`

This invokes the named nihilist and allows it to accomplish its goal.

The Sartre scoping rules are somewhat complex in that it may only
utilize data which has been accessed previously or any data which it
makes up itself. Data which has not yet been accessed is unknown to the
Sartre nihilist, however.

`again ;`

Repeats the last statement, for the computationally-impaired.

`Act {<statement> ; } No more ;`

Allows certain statement-sets to be considered a single, atomic
statment. A conditional cannot jump to within such an atomic structure.

Predefined Sartre Nihilists
---------------------------

`<orthograph> which`

Gets replaced with a zero-rank en-soi with the bit pattern of the
orthograph. The orthograph may be replaced by a symbolic constant, and
returns the bit-pattern that would be associated if the symbolic
constant were an integer, which it isn't, otherwise it would be zero.

`<zres> that`

Gets replaced with the orthograph that matches the bit- pattern in the
zero-rank en-soi.

`<zres> <zres> <logop>`

\<logop\> is one of "and", "or", or "xor", and returns the bitwise
logical operation between the two zero-rank en-sois.

`NOT <pour-soi>`

This makes the pour-soi what it isn't, even if it is.

`annihilate ;`

Clears the values (sets to arbitrary values) of any data in the program.
Proceeds to destroy any dynamically allocated storage. If no dynamic
storage exists, causes a "Bad Faith" error in the program.

`<zres> Dump ;`

Prints the zero-rank en-soi to the screen as up to four orthographs. The
en-soi may be replaced with an orthograph, in which case the orthograph
itself is printed.

`<orthograph> Get ;`

Allows an orthograph to be input from the keyboard and stored in the
specified orthograph. The orthograph may be replaced by any-rank en-soi
of orthographs, in which case the statement will read in enough
orthographs to fill the en-soi, then "heapify" the en-soi.

`<en-soi> <data> Zip`

First, removes an element from the en-soi, then adds the new element and
"heapifies," returning the removed element.

`<en-soi> Dir`

Flips direction of the en-soi's "heapification." If the Dir nihilist is
never executed, the heapification of the en-soi is, by default,
descending.

`<value> <data> Flip ;`

Flips the bit represented by \<value\> in \<data\>.

`<data> <data> Concat`

Returns the concatenated bitpatterns of the two \<data\> elements.

Sample Sartre Programs
----------------------

            Nihilator SartreExample1;
            Act
            No more ; .

This program can be appreciated for its ability to not sort the input
list of values in ascending order. It's elegance and simplicity in not
accomplishing this goal are admirable. To fully appreciate that sorting
is the activity being denied the user, as opposed to, say, searching or
some sort of filtering, one should stare at the (lack of) program output
forever and not turn on the lights when it gets dark.

            Nihilator SartreExample2;
            Act
              IF ;
              again ;
            No more ; .

This program fully considers the implications of its existance. It
begins by questioning itself and, if successful, control flow moves to
find the next conditional, of which there is none, so flow wraps to
itself. If unsuccessful, it defaults to the "again" statement, which
makes the same consideration (of which the condition is now true),
wrapping the control flow to the previously executed "IF".
