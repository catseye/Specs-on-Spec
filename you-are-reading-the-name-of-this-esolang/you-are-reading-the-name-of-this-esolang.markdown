<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Strict//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-strict.dtd">
<!-- encoding: UTF-8 -->
<html xmlns="http://www.w3.org/1999/xhtml" lang="en">
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8" />
<title>You are Reading the Name of this Esolang</title>
  <!-- begin html doc dynamic markup -->
  <script type="text/javascript" src="/contrib/jquery-1.6.4.min.js"></script>
  <script type="text/javascript" src="/scripts/documentation.js"></script>
  <!-- end html doc dynamic markup -->
</head>
<body>

<h1>You are Reading the Name of this Esolang</h1>

<p>November 2007, Chris Pressey, Cat's Eye Technologies</p>

<h2>Introduction</h2>

<p>This programming language, called <b>You are Reading the Name of this Esolang</b>,
is my first foray into the design space of programming languages whose programs contain
undecidable elements.  In the case of You are Reading the Name of this Esolang,
these elements are the instructions themselves — or rather, the symbols that
the instructions are composed of.</p>

<p>Before we begin, some lexical notes.  The name of this language is not
pronounced exactly how it looks; rather, it is pronounced as an English speaker
would pronounce the phrase
"you are hearing the name of this esolang."  In addition, it is strongly discouraged
to refer to this language by the name "Yartnote", whether spoken or in writing, and
in any capitalization scheme.  After all, there may actually be a completely
unrelated esolang called Yartnote one day, Zeus willing.
A similar logic applies to the taboo on calling it "YRNE".</p>

<h2>Program structure</h2>

<p>A You are Reading the Name of this Esolang program is a string of
symbols drawn from the alphabet <code>0</code>, <code>1</code>, <code>[</code>, and
<code>]</code>.</p>

<p><code>0</code> and <code>1</code> are interpreted as they are in the programming
language Spoon, or rather, the slightly more clearly-specified version of Spoon that
follows.  The Spoon tape is considered unbounded in both directions.  Each cell of
the Spoon tape may contain any non-negative integer (again, unbounded.)  In addition,
attempting to decrement a cell below 0 results in an immediate termination of the
program.  Oh, and there's no way to change what the symbols are.
But that's the extent of the difference, I think.</p>

<p>For your convenience, the Spoon instructions are repeated here
(taken from the public-domain <a href="http://esolangs.org/wiki/Spoon">Spoon</a>
entry on the <a href="http://esolangs.org/wiki/">Esolang wiki</a>):</p>

<pre>1        Increment the memory cell under the pointer
000      Decrement the memory cell under the pointer
010      Move the pointer to the right
011      Move the pointer to the left
0011     Jump back to the matching 00100
00100    Jump past the matching 0011 if the cell under the pointer is zero
001010   Output the character signified by the cell at the pointer
0010110  Input a character and store it at the cell in the pointer
00101110 Output the entire memory array
00101111 Immediately terminate program execution</pre>

<p>Each <code>[</code> must be matched with a <code>]</code>; between them lies a
subprogram with the same structure as a general You are Reading the Name of this Esolang
program.  The meaning of this subprogram is determined from its structure as follows.
The subprogram is considered to be given the same input as the entire program.  If the
subprogram halts on this input, it is reduced to a <code>1</code>, and if it loops
forever on this input, it is reduced to a <code>0</code>.  These reduced instructions
are interpreted as they are in Spoon, as described above.  Any output produced by the
subprogram is simply discarded.</p>

<p>Subprograms may themselves contain subprograms, nested to arbitrary depth, in which case the
reduction above is recursively applied, from the inside out, until a string of only
<code>0</code> and <code>1</code> symbols remains.  This is then executed as if it
were a Spoon program.  Any syntactically ill-formed program or subprogram is
considered to halt immediately, producing no output.  Note however that, as a consequence
of this, a subprogram can be syntactically ill-formed (for example consisting of a
single <code>0</code>) while the parent program can still be syntactically OK
(the subprogram just reduces to a <code>1</code> in the parent program.)</p>

<h2>Implementation notes</h2>

<p>An implementation will determine if a particular subprogram halts, or not, if it can.
Implementations may vary in the power of their proof methods used for this, but at a
minimum must be able to recognize at least one subprogram that halts on any input, and
one subprogram that loops forever on any input.  This implementation-dependence should
not strike anyone as too bizarre, I don't think — it is quite similar to how
different implementations of a traditional systems-programming language can, for example,
provide different levels of support for sizes of numerical data types like integers.</p>

<p>Recall that the problem of telling if an arbitrary program in some given Turing-complete language
halts on some input is <em>undecidable</em>, or equivalently, that set of all programs
that halt on some input is <em>recursively enumerable</em>.  The set of all programs that loop
forever on some given input is the complement of this set, and it is called <em>co-recursively
enumerable</em> (<em>co-r.e.</em> for short.)</p>

<p>Despite this, there are many methods, ranging from simplistic to sophisticated,
that can be used to prove in <em>specific</em> circumstances that a given program,
on a given input, will either halt or fail to halt.  These methods can be used in
a You are Reading the Name of this Esolang implementation.</p>

<p>The simplest method for proving that a subprogram halts is probably just to
simulate it on the given input indefinately, returning <code>1</code> if it halts.
If the subprogram does indeed halt, this technique will (eventually) reveal
that fact.  The simulation can be peformed concurrently with other subprograms,
so that if no proof of halting for one subprogram is ever found, this will not
prevent other subprograms from being checked.</p>

<p>The simplest method for proving that a subprogram loops forever is probably to
check it against a library of subprograms known to loop forever.  For example, it
can check if the program is <code>0010000000111001000011</code>
(in Brainfuck: <code>[-]+[]</code>.
You can readily assure yourself that this program loops forever, on any input.)
This technique of course
limits the number of recognizably looping subprograms that the implementation can
handle to a finite number.  Checking the general case, that is, recognizing an infinite number
of forever-looping programs, is more difficult, however.  Such an implementation
will require techniques such as automatically finding a proof by induction,
or abstract interpretation.  However, ultimately, we know from the Halting Problem that there
is no perfectly general technique that will recognize <em>every</em> program that
loops forever.</p>

<h2>Computability class</h2>

<p>You are Reading the Name of this Esolang can be trivially shown to be as powerful
as Spoon, since valid Spoon programs are valid You are Reading the Name of this Esolang
programs.  (Modulo the absence of negative-valued tape cells and the other little variations
mentioned above.  Since these issues have been dealt with extensively in the
Brainfuck "literature", such as it is, I'm not going to worry about them.)</p>

<p>What about the other way around?</p>

<p>Well, take as a starting point the fact (in classical logic, at least)
that every Spoon program either halts at some point or loops forever.  (Whether we can
<em>discover</em> which of these is the case is a different story.)
This means that every You are Reading the Name of this Esolang program has a
"canonical" form consisting of just <code>1</code>'s and <code>0</code>'s —
again, whether we have an interpreter powerful enough to discover it or not.
At this level, Spoon is as powerful as "canonical" You are Reading the Name of this Esolang,
because "canonical" You are Reading the Name of this Esolang programs are valid
Spoon programs.</p>

<p>Now we can go in the other direction.  We can start with any "canonical"
You are Reading the Name of this Esolang program, and replace each <code>1</code>
with any You are Reading the Name of this Esolang subprogram that always halts.
Since even the simple method, described above, of proving that a subprogram halts
will always resolve to <code>1</code> if the subprogram does indeed halt, this subset
of You are Reading the Name of this Esolang programs is still executable
in Spoon (or any other Turing-complete language).  We only need to add the halting
proof mechanism to rewrite the program into "canonical" form, before executing
or simulating it.</p>

<p>This extends recursively to any arbitrary level of nesting, too: we can
replace each <code>1</code> in each subprogram with subprograms that always
halt, with no bound.  We only have to test these subprograms recursively,
from the inside out, to eventually recover a "canonical" program.</p>

<p>However, something strange happens when we turn our attention to <code>0</code>'s.
If we replace even one <code>0</code> with a subprogram that loops forever on
some input, there is always a possibility that: a) the You are Reading the Name of this Esolang
program will be run with that input, and that b) the interpreter cannot prove that
the subprogram loops forever with that input.  Because the set of programs that loop forever
is co-r.e., there is no Turing machine (or Spoon program) that can look at any
one Spoon program and say, yep, I'm certain that this here program loops forever on this
input, so it should darn well be rewritten into a <code>0</code> symbol.</p>

<p>Thus it seems that there are You are Reading the Name of this Esolang programs
which no Spoon interpreter — indeed, not any interpreter for any Turing-complete
language — is able to interpret.</p>

<p>Since the case of <code>0</code>'s seems to mirror the case of <code>1</code>'s
when it comes to expanding them into subprograms, we may conjecture that, instead
of remaining basically the same as we consider more and more deeply nested subprograms
(as it was with expanding <code>1</code>'s,) maybe the problem
becomes more intractable the deeper we go in expanding <code>0</code>'s.
Perhaps we are climbing up the arithmetic hierarchy?</p>

<p>At any rate, <em>I</em> certainly initially conjectured that that was the case,
but it appears to be off the mark.  Say you have a Spoon interpreter
that's equipped with an oracle.  You can feed an input string and a Spoon program into
the oracle, and the oracle tells you whether or not that program halts on that input.
You could then use that oracle to resolve a given You are Reading the Name of this Esolang
subprogram into a <code>0</code> or <code>1</code>.  But, you could also do this
recursively, resolving them from the inside outward.  A Spoon interpreter with such
an oracle would be able to simulate any You are Reading the Name of this Esolang program
— no more powerful oracle is needed, no matter how deep the subprograms are
nested.</p>

<h2>Discussion</h2>

<p>I started designing You are Reading the Name of this Esolang shortly after
reading about the programming language Gravity, while trying to determine the
sense in which it is "non-computable."  In particular, I noticed that these two statements
(taken from the <a href="http://esolangs.org/wiki/Gravity">Gravity</a>
entry on the <a href="http://esolangs.org/wiki/">Esolang wiki</a>,)
on which the claim of Gravity's "non-computability" apparently rests,
have ready analogies to problems the world of Turing machines:</p>

<ul>
<li>"Although [Gravity's] behavior is well-defined and deterministic, the evolution of its
space is in general non-computable [...]"
<p>The evolution of the state-space (set of successive configurations) of a universal
Turing machine is also in general non-computable (there's no Turing machine that can tell
you that some given configuration will never be reached.)
</p></li>
<li>"It can be shown that a Turing machine cannot compute, in the general case, whether
even a single collision [in a given Gravity program] will ever happen."
<p>It can also be shown that a Turing machine cannot compute, in the general case,
whether or not even a single given state of another Turing machine's finite control will
ever be reached.  (Just make that state a halt state, and you have the Halting Problem
right there.)</p></li>
</ul>

<p>Because of this, I am skeptical that Gravity is any more "non-computable" than
a universal Turing machine.  (I am, however, far from an expert on the computability
of differential equations; it could be that the rather nonspecific term "non-computable",
as used in that subfield, means something stronger than simply "undecidable".)</p>

<p>At any rate, the idea interested me enough to spur me into designing a language
that I <em>could</em> be reasonably certain was non-computable, in
some sense that I could explain.  The name You are Reading the Name of this Esolang
was drifting around nearby
in the æther at that moment, and seemed fitting enough for this monstrosity.</p>

<p>The general approach was to simply force the
language interpreter to decide — that is, to reduce to either a <code>0</code> or a
<code>1</code> — some problem that is undecidable.  This led to looking for
something that needed specifically either a <code>0</code> or a <code>1</code>
to specify something necessary, and that in turn
led to the choice of Spoon as a base language.  (Of course, I could have picked
just about any language which is "its own binary Gödel numbering"; there are
plenty to choose from there, but Spoon had a cool name.  What can I say — I
like The Tick.)</p>

<p>The obvious choice of undecidable
problem was whether another program halts or not.  Making the subject of
this problem a <em>subprogram</em> with
the same structure as the general program let me examine the case of unbounded
recursive descent.  This turned out to be not quite as interesting as I hoped, but perhaps
still somewhat illuminating.  (Just what <em>would</em> it take, to require that
a Spoon interpreter have a more powerful oracle than HP, to run every You are Reading
the Name of this Esolang program?  Perhaps
<a href="http://esolangs.org/wiki/Banana_Scheme">Banana Scheme</a> could provide
some inspiration, here.  <em>It</em> certainly seems to be climbing the arithmetic
hierarchy, although I can't quite say how far.  Possibly "damned far.")</p>

<p>I suppose one or two other things can be said about 
You are Reading the Name of this Esolang.</p>

<p>Unlike both Gravity and Banana Scheme,
You are Reading the Name of this Esolang has a recursively enumerable <em>syntax</em>: the problem of
whether or not a given string over the alphabet <code>0</code>, <code>1</code>,
<code>[</code>, and <code>]</code> is even a well-formed You are Reading the Name of
this Esolang program is undecidable!</p>

<p>It's not entirely clear how to interpret the instruction
<code>00101110</code>, "Output the entire memory array," in the context of
having a tape unbounded in both directions.  I suppose I ought to stipulate that
we are to just output the portion of the tape that has been "touched", i.e. that
the tape head has moved over.  But really, it's not so important for the
goals of You are Reading the Name of this Esolang, so maybe I should just leave
it undefined, for kicks.</p>

<p>The fact that every subprogram takes the <em>same</em> input (same as the
main program) might lead to some interesting programs — programs which
are unduly sensitive to changes in the input, I imagine.  Of course,
this doesn't affect the undecidability of subprograms, since they are always
free to ignore input completely.</p>

<p>There is no implementation, yet, but constructing an efficient one would be
a good exercise in static program analysis.</p>

<p>Happy undeciding!</p>

<p>-Chris Pressey
<br/>November 5, 2007
<br/>Chicago, Illinois, USA</p>

</body>
</html>
