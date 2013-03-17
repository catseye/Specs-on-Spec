Multi-Directional Pattern Notation
==================================

Final - Sep 6 1999

* * * * *

Introduction
------------

MDPN is an extension to EBNF, which attributes it for the purposes of
scanning and parsing input files which assume a non-unidirectional form.
A familiarity with EBNF is assumed for the remainder of this document.

MDPN was developed by Chris Pressey in late 1998, built on an earlier,
less successful attempt at a "2D EBNF" devised to fill a void that the
mainstream literature on parsing seemed to rarely if ever approach, with
much help provided by John Colagioia throughout 1998.

MDPN has possible uses in the construction of parsers and subsequently
compilers for multi-directional and multi-dimensional languages such as
Orthogonal, Befunge, Wierd, Blank, Plankalkül, and even less contrived
notations like structured Flowchart and Object models of systems.

As the name indicates, MDPN provides a notation for describing
multidimensional patterns by extending the concept of linear scanning
and matching with geometric attributes in a given number of dimensions.

Preconditions for Multidirectional Parsing
------------------------------------------

The multidirectional parsing that MDPN concerns itself with assumes that
any portion of the input file is accessable at any time. Concepts such
as LL(1) are fairly meaningless in a non-unidirectional parsing system
of this sort. The unidirectional input devices such as paper tape and
punch cards that were the concern of original parsing methods have been
superceded by modern devices such as hard disk drives and ample, cheap
RAM.

In addition, MDPN is limited to an orthogonal representation of the
input file, and this document is generally less concerned about forms of
four or higher dimensions, to reduce unnecessary complexity.

Notation from EBNF
------------------

Syntax is drawn from EBNF. It is slightly modified, but should not
surprise anyone who is familiar with EBNF.

A freely-chosen unadorned ('bareword') alphabetic multicharacter
identifier indicates the name of a nonterminal (named pattern) in the
grammar. e.g. `foo`. (Single characters have special meaning as
operators.) Case matters: `foo` is not the same name as `Foo` or `FOO`.

Double quotes begin and end literal terminals (symbols.) e.g. `"bar"`.

A double-colon-equals-sign (`::=`) describes a production (pattern
match) by associating a single nonterminal on the left with a pattern on
the right, terminated with a period. e.g. `foo ::= "bar".`

A pattern is a series of terminals, nonterminals, operators, and
parenthetics.

The `|` operator denotes alternatives. e.g. `"foo" | "bar"`

The `(` and `)` parentheses denote precedence and grouping.

The `[` and `]` brackets denote that the contents may be omitted, that
is, they may occur zero or one times. e.g. `"bar" ["baz"]`

The `{` and `}` braces denote that the contents may be omitted or may be
repeated any number of times. e.g. `"bar" {baz "quuz"}`

Deviations from EBNF
--------------------

The input file is spatially related to a coordinate system and it is
useful to think of the input mapped to an orthogonally distributed
(Cartesian) form with no arbitrary limit imposed on its size,
hereinafter referred to as *scan-space*.

The input file is mapped to scan-space. The first printable character in
the input file always maps to the *origin* of scan-space regardless of
the number of dimensions. The origin is enumerated with coordinates (0)
in one dimension, (0,0) in two dimensions, (0,0,0) in three dimensions,
etc.

Scan-space follows the 'computer storage' co-ordinate system so that *x*
coordinates increase to the 'east' (rightwards), *y* coordinates
increase to the 'south' (downwards), and *z* coordinates increase on
each successive 'page'.

Successive characters in the input file indicate successive coordinate
(*x*) values in scan-space. For two and three dimensions, end-of-line
markers are assumed to indicate "reset the *x* dimension and increment
the *y* dimension", and end-of-page markers indicate "reset the *y*
dimension and increment the *z* dimension", thus following the
commonplace mapping of computer text printouts.

Whitespace in the input file are **not** ignored. The terminal `" "`,
however, will match any whitespace (including tabs, which are **not**
expanded.) The pattern `{" "}` may be used to indicate any number of
whitespaces; `" " {" "}` may be used to indicate one or more
whitespaces. Areas of scan-space beyond the bounds of the input file are
considered to be filled with whitespaces.

Therefore, `"hello"` as a terminal is exactly the same as
`"h" "e" "l" "l" "o"` as an pattern of terminals.

A `}` closing brace can be followed by a `^` (*constraint*) operator,
which is followed by an expression in parentheses.

This expression is actually in a subnotation which supports a very
simple form of algebra. The expression (built with terms connected by
infix `+-*/%` operators with their C language meanings) can either
reduce to

-   a constant value, as in `{"X"} ^ (5)`, which would match five `X`
    terminals in a line; or
-   an unknown value, which can involve any single lowercase letters,
    which indicate variables local to the production, as in
    `{"+"}^(x) {"-"}^(x*2)`, which would match only twice as many minus
    signs as plus signs.

Complex algebraic expressions in constraints can and probably should be
avoided when constructing a MDPN grammar for a real (non-contrived)
compiler. MDPN-based compiler-compilers aren't expected to support more
than one or two unknowns per expression, for example. There is no such
restriction, of course, when using MDPN as a guide for hand-coding a
multidimensional parser, or otherwise using it as a more sophisticated
pattern-matching tool.

The Scan Pointer
----------------

It is useful to imagine a *scan pointer* (SP, not to be confused with a
*stack pointer*, which is not the concern of this document) which is
analogous to the current token in a single-dimensional parser, but
exists in MDPN as a free spatial relationship to the input file, and
thus also has associated geometric attributes such as direction.

The SP's *location* is advanced through scan-space by its *heading* as
terminals in the productions are successfully matched with symbols in
the input buffer.

The following geometric attribution operators modify the properties of
the SP. Note that injudicious use of any of these operators *can* result
in an infinite loop during scanning. There is no built-in contingency
measure to escape from an infinite parsing loop in MDPN (but see
exclusivity, below, for a possible way to overcome this.)

`t` is the relative translation operator. It is followed by a vector, in
parentheses, which is added to the location of the SP. This does not
change its heading.

For example, `t (0,-1)` moves the SP one symbol above the current symbol
(the symbol which was *about* to be matched.)

As a more realistic example of how this works, consider that the pattern
`"." t(-1,1) "!" t(0,-1)` will match a period with an exclamation point
directly below it, like:

    .
    !

`r` is the relative rotation operator. It is followed by an axis
identifier (optional: see below) and an orthogonal angle (an angle *a*
such that |*a*| **mod** 90 degrees = 0) assumed to be measured in
degrees, both in parentheses. The angle is added to the SP's heading.
Negative angle arguments are allowed.

Described in two dimensions, the (default) heading 0 denotes 'east,'
that is, parsing character by character in a rightward direction, where
the SP's *x* axis coordinate increases and all other axes coordinates
stay the same. Increasing angles ascend counterclockwise (90 = 'north',
180 = 'west', 270 = 'south'.)

For example, `">" r(-90) "+^"` would match

    >+
     ^

The axis identifier indicates which axis this rotation occurs around. If
the axis identifier is omitted, the *z* axis is to be assumed, since
this is certainly the most common axis to rotate about, in two
dimensions.

If the axis identifier is present, it may be a single letter in the set
`xyz` (these unsurprisingly indicate the *x*, *y*, and *z* dimensions
respectively), or it may be a non-negative integer value, where 0
corresponds to the *x* dimension, 1 corresponds to the *y* dimension,
etc. (Implementation note: in more than two dimensions, the SP's heading
property should probably be broken up internally into theta, rho, &c
components as appropriate.)

For example, `r(z,180)` rotates the SP's heading 180 degrees about the
*z* (dimension \#2) axis, as does `r(2,180)` or even just `r(180)`.

`<` and `>` are the push and pop state-stack operators, respectively.
Alternately, they can be viewed as lookahead-assertion parenthetics,
since the stack is generally assumed to be local to the production.
(Compiler-compilers should probably notify the user, but not necessarily
panic, if they find unbalanced `<>`'s.)

All properties of the SP (including location and heading, and scale
factor if supported) are pushed as a group onto the stack during `<` and
popped as a group off the stack during `>`.

Advanced SP Features
--------------------

These features are not absolutely necessary for most non-contrived
multi-directional grammars. MDPN compiler-compilers are not expected to
support them.

`T` is the absolute translation operator. It is followed by a vector
which is assigned to the location of the SP. e.g. `T (0,0)` will 'home'
the scan.

`R` is the absolute rotation operator. It is followed by an optional
axis identifier, and an orthogonal angle assumed to be measured in
degrees. The SP's heading is set to this angle. e.g. `R(270)` sets the
SP scanning line after line down the input text, downwards. See the `r`
operator, above, for how the axis identifier functions.

`S` is the absolute scale operator. It is followed by an orthogonal
*scaling factor* (a scalar *s* such that *s* = **int**(*s*) and *s* \>=
1). The SP's scale factor is set to this value. The finest possible
scale, 1, indicates a 1:1 map with the input file; for each one input
symbol matched, the SP advances one symbol in its path. When the scale
factor is two, then for each one input symbol matched, the SP advances
two symbols, skipping over an interim symbol. Etc.

`s` is the relative scale operator. It is followed by a scalar integer
which is added to the SP's scaling factor (so long as it does not cause
the scaling factor to be zero or negative.)

Scale operators may also take an optional axis identifier (as in
`S(y,2)`), but when the axis identifier is omitted, all axes are assumed
(non-distortional scaling).

`!>` is a state-assertion alternative to `>`, for the purpose of
determining that the SP successfully and completely reverted to a
previous state that was pushed onto the stack ('came full circle'). This
operator is something of a luxury; a grammar which uses constraints
correctly should never *need* it, but it can come in handy.

Other Advanced Features: Exclusivity
------------------------------------

Lastly, in the specification of a production, the *exclusivity* applying
to that production can be given between a hyphen following the name of
the nonterminal, and the `::=` operator.

Exclusivity is a list of productions, named by their nonterminals, and
comes into play at any particular *instance* of the production (i.e.
when the production successfully matches specific symbols at specific
points in scan-space during a parse, called the *domain*.) The
exclusivity describes how the domain of each instance is protected from
being the domain of any further instances. The domain of any subsequent
instances of any productions listed in the exclusivity is restricted
from sharing points in scan-space with the established domain.

Exclusivity is a measure to prevent so-called *crossword grammars* -
that is, where instances of productions can *overlap* and share common
symbols - if desired. Internally it's generally considered a list of
'used-by-this-production' references associated with each point in
scan-space. An example of the syntax to specify exclusivity is
`bar - bar quuz ::= foo {"a"} baz`. Note that the domain of an instance
of `bar` is the sum of the domains `foo`, `baz` and the chain of "`a`"
terminals, and that neither a subsequent instance of `quuz` nor `bar`
again can overlap it.

Examples of MDPN-described Grammars
-----------------------------------

**Example 1.** A grammar for describing boxes.

The task of writing a translator to recognize a two-dimensional
construct such as a box can easily be realized using a tool such as
MDPN.

An input file might contain a of box drawn in ASCII characters, such as

    +------+
    |      |
    |      |
    +------+

Let's also say that boxes have a minimum height of four (they must
contain at least two rows), but no minimum width. Also, edge characters
must match up with which edge they are on. So, the following forms are
both illegal inputs:

    +-+
    +-+

    +-|-+
    |   |
    *    
    |   |
    +-|-+

The MDPN production used to describe this box might be

      Box ::= "+" {"-"}^(w) r(-90) "+" "||" {"|"}^(h) r(-90)
              "+" {"-"}^(w) r(-90) "+" "||" {"|"}^(h) r(-90).

**Example 2.** A simplified grammar for Plankalkül's assignments.

An input file might contain an ASCII approximation of something Zuse
might have jotted down on paper:

     |Z   + Z   => Z
    V|1     2      3
    S|1.n   1.n    1.n

Simplified MDPN productions used to describe this might be

    Staff     ::= Spaces "|" TempVar AddOp TempVar Assign TempVar.
    TempVar   ::= "Z" t(-1,1) Index t(-1,1) Structure t(0,-2) Spaces.
    Index     ::= <Digit {Digit}>.
    Digit     ::= "0" | "1" | "2" | "3" | "4" | "5" | "6" | "7" | "8" | "9".
    Structure ::= <Digit ".n">.
    AddOp     ::= ("+" | "-") Spaces.
    Assign    ::= "=>" Spaces.
    Spaces    ::= {" "}.
