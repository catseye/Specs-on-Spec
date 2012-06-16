<html>
<head>
  <meta http-equiv="Content-Type" content="text/html; charset=UTF-8" />
  <meta name="Description" content="Cat's Eye Technologies: MDPN: Multi-Dimensional Pattern Notation">
  <title>Cat's Eye Technologies: MDPN: Multi-Dimensional Pattern Notation</title>
  <!-- begin html doc dynamic markup -->
  <script type="text/javascript" src="/contrib/jquery-1.6.4.min.js"></script>
  <script type="text/javascript" src="/scripts/documentation.js"></script>
  <!-- end html doc dynamic markup -->
</head>
<body>

<h1>Multi-Directional Pattern Notation</h1>
<p>Final - Sep 6 1999</p>
<hr>

<h2>Introduction</h2>

<p>MDPN is an extension to EBNF, which attributes it for
the purposes of scanning and parsing input files which
assume a non-unidirectional form.  A familiarity with
EBNF is assumed for the remainder of this document.</p>

<p>MDPN was developed by Chris Pressey in late 1998,
built on an earlier, less successful attempt at a "2D EBNF" devised
to fill a void that the 
mainstream literature on parsing seemed to rarely if
ever approach, with much help provided by John Colagioia
throughout 1998.</p>

<p>MDPN has possible uses in the construction of
parsers and subsequently compilers for multi-directional and
multi-dimensional languages such as Orthogonal, Befunge,
Wierd, Blank, Plankalk&uuml;l, and even less contrived notations
like structured Flowchart and Object models of systems.</p>

<p>As the name indicates, MDPN provides a notation for
describing multidimensional patterns by
extending the concept of linear scanning and matching 
with geometric attributes in a given number of dimensions.</p>

<h2>Preconditions for Multidirectional Parsing</h2>

<p>The multidirectional parsing that MDPN concerns itself
with assumes that any portion of the input file is accessable
at any time.  Concepts such as LL(1) are fairly meaningless
in a non-unidirectional parsing system of this sort.
The unidirectional input devices such as paper tape and
punch cards that were the concern of original parsing methods
have been superceded
by modern devices such as hard disk drives and ample, cheap RAM.</p>

<p>In addition, MDPN is limited to an orthogonal
representation of the input file, and this
document is generally less concerned about forms of
four or higher dimensions, to reduce unnecessary complexity.</p>

<h2>Notation from EBNF</h2>

<p>Syntax is drawn from EBNF.  It is slightly modified, but
should not surprise anyone who is familiar with EBNF.</p>

<p>A freely-chosen unadorned ('bareword') alphabetic
multicharacter identifier indicates
the name of a nonterminal (named pattern) in the
grammar.  e.g. <code>foo</code>.  (Single characters have special
meaning as operators.)  Case matters: <code>foo</code> is not the
same name as <code>Foo</code> or <code>FOO</code>.</p>

<p>Double quotes begin and end literal terminals (symbols.)
e.g. <code>"bar"</code>.</p>

<p>A double-colon-equals-sign (<code>::=</code>)
describes a production (pattern match)
by associating a single nonterminal on the left with
a pattern on the right, terminated with a period.
  e.g. <code>foo ::= "bar".</code></p>

<p>A pattern is a series of terminals, nonterminals,
operators, and parenthetics.</p>

<p>The <code>|</code> operator denotes
alternatives.  e.g. <code>"foo" | "bar"</code></p>

<p>The <code>(</code> and <code>)</code> parentheses denote
precedence and grouping.</p>

<p>The <code>[</code> and <code>]</code> brackets denote
that the contents may be omitted, that is, they
may occur zero or one times.  e.g. <code>"bar" ["baz"]</code></p>

<p>The <code>{</code> and <code>}</code> braces denote
that the contents may be omitted or may be repeated any number of times.
e.g. <code>"bar" {baz "quuz"}</code></p>

<h2>Deviations from EBNF</h2>

<p>The input file is spatially related to a
coordinate system and it is useful to
think of the input mapped to an orthogonally
distributed (Cartesian) form with no arbitrary
limit imposed on its size, hereinafter referred to
as <i>scan-space</i>.</p>

<p>The input file is mapped to scan-space.  The
first printable character in the input file always
maps to the <i>origin</i> of scan-space regardless of
the number of dimensions.  The origin is enumerated
with coordinates (0) in one dimension, (0,0) in two
dimensions, (0,0,0) in three dimensions, etc.</p>

<p>Scan-space follows the 'computer storage' co-ordinate
system so that <i>x</i> coordinates increase to
the 'east' (rightwards), <i>y</i> coordinates increase to the
'south' (downwards), and <i>z</i> coordinates increase on each
successive 'page'.</p>

<p>Successive characters in the input file indicate
successive coordinate (<i>x</i>) values in scan-space.
For two and three dimensions, end-of-line markers are assumed
to indicate "reset the <i>x</i> dimension and increment
the <i>y</i> dimension", and end-of-page markers
indicate "reset the <i>y</i> dimension and increment
the <i>z</i> dimension",
thus following the
commonplace mapping of computer text printouts.</p>

<p>Whitespace in the input file are <b>not</b> ignored.
The terminal <code>" "</code>, however, will match any
whitespace (including tabs, which are <b>not</b> expanded.)
The pattern <code>{" "}</code> may be used to indicate
any number of whitespaces; <code>" " {" "}</code> may be used to
indicate one or more whitespaces.  Areas of scan-space
beyond the bounds of the input file are considered to be filled
with whitespaces.</p>

<p>Therefore, <code>"hello"</code> as a terminal is exactly
the same as <code>"h" "e" "l" "l" "o"</code> as an pattern of
terminals.</p>

<p>A <code>}</code> closing brace can be followed by a <code>^</code>
(<i>constraint</i>)
operator, which is followed by an expression in parentheses.</p>

<ul>
<p>This expression is actually in a subnotation which supports
a very simple form of algebra.  The expression (built with
terms connected by infix <code>+-*/%</code> operators with their
C language meanings) can either reduce to
<ul>
<li>a constant value, as in <code>{"X"} ^ (5)</code>, which
would match five <code>X</code> terminals in a line; or
<li>an unknown value, which can involve any single lowercase
letters, which indicate variables local to the production,
as in <code>{"+"}^(x) {"-"}^(x*2)</code>, which would match only twice
as many minus signs as plus signs.
</ul></p>

<p>Complex algebraic expressions in constraints can and probably
should be avoided when constructing a MDPN grammar
for a real (non-contrived) compiler.  MDPN-based
compiler-compilers aren't expected to support more than
one or two unknowns per expression, for example.
There is no such restriction, of course,
when using MDPN as a guide for hand-coding a multidimensional
parser, or otherwise using it as a more 
sophisticated pattern-matching tool.</p>
</ul>

<h2>The Scan Pointer</h2>

<p>It is useful to imagine a <i>scan pointer</i> (SP,
not to be confused with a <i>stack pointer</i>, which is not
the concern of this document) which is analogous to the
current token in a single-dimensional parser, but exists
in MDPN as a free spatial relationship to the input file, and
thus also has associated geometric attributes such as
direction.</p>

<p>The SP's <i>location</i> is advanced through scan-space by its
<i>heading</i> as terminals in the productions
are successfully matched with symbols in the input buffer.</p>

<p>The following geometric attribution operators modify the
properties of the SP.  Note that injudicious
use of any of these operators <i>can</i> result in an infinite loop
during scanning.  There is no built-in contingency measure to
escape from an infinite parsing loop in MDPN (but see
exclusivity, below, for a possible way to overcome this.)</p>

<p><code>t</code> is the relative translation operator.  It is
followed by a vector, in parentheses, which is added to the
location of the SP.  This does not change its heading.</p>

<p>For example, <code>t (0,-1)</code> moves the
SP one symbol above the current symbol (the symbol which was
<i>about</i> to be matched.)</p>

<p>As a more realistic example of how this works, consider that the
pattern <code>"." t(-1,1) "!" t(0,-1)</code> will match a
period with an exclamation point directly below it, like:

<pre>.
!</pre>
</p>

<p><code>r</code> is the relative rotation operator.  It is
followed by an axis identifier (optional: see below)
and an orthogonal angle (an angle <i>a</i>
such that |<i>a</i>| <b>mod</b> 90 degrees = 0) assumed to
be measured in degrees, both in parentheses.
The angle is added to the SP's heading.
Negative angle arguments are allowed.</p>

<p>Described in two dimensions, the (default) heading 0 denotes 'east,' that
is, parsing character by character in a rightward direction, where
the SP's <i>x</i> axis coordinate increases and all other axes coordinates
stay the same. Increasing angles ascend counterclockwise (90 = 'north',
180 = 'west', 270 = 'south'.)</p>

<p>For example, <code>">" r(-90) "+^"</code> would match

<pre>&gt;+
 ^</pre>
</p>

<p>The axis identifier indicates which axis this rotation
occurs around.  If the axis identifier is omitted,
the <i>z</i> axis is to be assumed, since this is certainly the
most common axis to rotate about, in two dimensions.</p>

<p>If the axis identifier is present, it may be a single letter
in the set <code>xyz</code> (these unsurprisingly indicate
the <i>x</i>, <i>y</i>, and <i>z</i> dimensions respectively),
or it may be a non-negative integer value, where 0 corresponds to the
<i>x</i> dimension, 1 corresponds to the <i>y</i> dimension, etc.
(Implementation note: in more than two dimensions,
the SP's heading property should probably be broken up
internally into theta, rho, &amp;c components as appropriate.)</p>

<p>For example, <code>r(z,180)</code> rotates the SP's heading
180 degrees about the <i>z</i> (dimension #2) axis, as does <code>r(2,180)</code>
or even just <code>r(180)</code>.</p>

<p><code>&lt;</code> and <code>&gt;</code> are the push
and pop state-stack operators, respectively.  Alternately,
they can be viewed as lookahead-assertion parenthetics, since the stack
is generally assumed to be
local to the production.  (Compiler-compilers should probably
notify the user, but not necessarily panic, if they find unbalanced 
<code>&lt;&gt;</code>'s.)</p>

<p>All properties of the SP (including location and heading,
and scale factor if supported) are pushed as a group
onto the stack during <code>&lt;</code> and popped as a group
off the stack during <code>&gt;</code>.</p>

<h2>Advanced SP Features</h2>

<p>These features are not absolutely necessary for most
non-contrived multi-directional grammars.  MDPN compiler-compilers
are not expected to support them.</p>

<p><code>T</code> is the absolute translation operator.  It is
followed by a vector which is assigned to the location of the SP.
 e.g. <code>T (0,0)</code> will 'home' the scan.</p>

<p><code>R</code> is the absolute rotation operator.  It is
followed by an optional axis identifier, and an
orthogonal angle assumed to be measured in degrees.
The SP's heading is set to this
angle.  e.g. <code>R(270)</code> sets the
SP scanning line after line down the input text, downwards.  See
the <code>r</code> operator, above, for how the axis identifier functions.</p>

<p><code>S</code> is the absolute scale operator.  It is
followed by an orthogonal <i>scaling factor</i> (a scalar <i>s</i>
such that <i>s</i> = <b>int</b>(<i>s</i>) and <i>s</i> >= 1).
The SP's scale factor is set to this value.  The finest
possible scale, 1, indicates a 1:1 map with the input file;
for each one input symbol matched, the SP advances one symbol
in its path.  When the scale factor is two, then for each
one input symbol matched, the SP advances two symbols, skipping
over an interim symbol.  Etc.</p>

<p><code>s</code> is the relative scale operator.  It is
followed by a scalar integer which is added to the SP's scaling factor
(so long as it does not cause the scaling factor to be zero or negative.)</p>

<p>Scale operators may also take an optional axis identifier
(as in <code>S(y,2)</code>), but when the axis identifier is omitted,
all axes are assumed (non-distortional scaling).</p>

<p><code>!&gt;</code> is a state-assertion alternative to
<code>&gt;</code>, for the purpose of determining that
the SP successfully and completely
reverted to a previous state that
was pushed onto the stack ('came full circle').  This
operator is something of a luxury; a
grammar which uses constraints correctly should
never <i>need</i> it, but it can come in handy.</p>

<h2>Other Advanced Features: Exclusivity</h2>

<p>Lastly, in the specification of a production,
the <i>exclusivity</i> applying to that production can be given
between a hyphen following
the name of the nonterminal, and the <code>::=</code> operator.</p>

<p>Exclusivity is a list of productions, named by their
nonterminals, and comes into play at any particular <i>instance</i>
of the production (i.e. when the production successfully
matches specific symbols at specific points in scan-space during a parse,
called the <i>domain</i>.)  The exclusivity describes how the domain of each
instance is protected from being the domain of any further instances.
The domain of any subsequent instances of any productions listed
in the exclusivity is restricted from sharing points in scan-space
with the established domain.</p>

<p>Exclusivity is a measure to prevent so-called <i>crossword
grammars</i> - that is, where instances of productions
can <i>overlap</i> and share common symbols - if desired.
Internally it's generally considered a list of
'used-by-this-production' references
associated with each point in scan-space.
An example of the syntax to specify exclusivity is
<code>bar - bar quuz ::= foo {"a"} baz</code>.
Note that the domain of an instance of <code>bar</code> is the
sum of the
domains <code>foo</code>, <code>baz</code> and the chain of "<code>a</code>" terminals,
and that neither a subsequent instance of <code>quuz</code> nor <code>bar</code>
again can overlap it.</p>

<h2>Examples of MDPN-described Grammars</h2>

<p><b>Example 1.</b>  A grammar for describing boxes.</p>

<p>The task of writing a translator to recognize a
two-dimensional construct such as a box can easily
be realized using a tool such as MDPN.</p>

<p>An input file might contain a of box drawn in
ASCII characters, such as

<pre>+------+
|      |
|      |
+------+</pre>
</p>

<p>Let's also say that boxes have a minimum height of four (they
must contain at least two rows), but
no minimum width.  Also, edge characters must match up with
which edge they are on.  So, the following forms are both
illegal inputs:

<pre>+-+
+-+</pre>

<pre>+-|-+
|   |
*    
|   |
+-|-+</pre>
</p>

<p>The MDPN production used to describe this box might be

<pre>  Box ::= "+" {"-"}^(w) r(-90) "+" "||" {"|"}^(h) r(-90)
          "+" {"-"}^(w) r(-90) "+" "||" {"|"}^(h) r(-90).</pre>
</p>

<p><b>Example 2.</b>  A simplified grammar for Plankalk&uuml;l's assignments.</p>

<p>An input file might contain an ASCII approximation of something
Zuse might have jotted down on paper:

<pre> |Z   + Z   => Z
V|1     2      3
S|1.n   1.n    1.n</pre>
</p>

<p>Simplified MDPN productions used to describe this might be

<pre>
Staff     ::= Spaces "|" TempVar AddOp TempVar Assign TempVar.<br>
TempVar   ::= "Z" t(-1,1) Index t(-1,1) Structure t(0,-2) Spaces.
Index     ::= &lt;Digit {Digit}&gt;.
Digit     ::= "0" | "1" | "2" | "3" | "4" | "5" | "6" | "7" | "8" | "9".
Structure ::= &lt;Digit ".n"&gt;.
AddOp     ::= ("+" | "-") Spaces.
Assign    ::= "=>" Spaces.
Spaces    ::= {" "}.
</pre>
</p>

<hr>
<i>Last Updated Mar 2&nbsp;<img src="/images/icons/copyright.gif"
   align=absmiddle width=12 height=12 alt="(c)" border=0>2004 <a
   href="/">Cat's Eye Technologies</a>.
All rights reserved.
This document may be freely redistributed in its unchanged form.</i>
</body></html>
