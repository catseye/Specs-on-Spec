<?xml version="1.0"?>
<!-- encoding: utf-8 -->
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Strict//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-strict.dtd">
<html xmlns="http://www.w3.org/1999/xhtml" lang="en">
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8" />
<title>The Oozlybub and Murphy Programming Language</title>
  <!-- begin html doc dynamic markup -->
  <script type="text/javascript" src="/contrib/jquery-1.6.4.min.js"></script>
  <script type="text/javascript" src="/scripts/documentation.js"></script>
  <!-- end html doc dynamic markup -->
</head>
<body>

<h1>Oozlybub and Murphy</h1>

<p>Language version 1.1</p>

<h2>Overview</h2>

<p>This document describes a new programming language.
The name of this language is <dfn>Oozlybub and Murphy</dfn>.
Despite appearances, this name refers to a single language.
The majority of the language is
named Oozlybub.  The fact that the language is not entirely named
Oozlybub is named Murphy.  Deal with it.</p>

<p>For the sake of providing an "olde tyme esoterickal de-sign", the
language combines several unusual features, including multiple interleaved parse
streams, infinitely long variable names, gratuitously strong typing, and
only-conjectural Turing completeness.
While no implementation of the language exists as of this writing, it is thought to
be sufficiently consistent to be implementable, modulo any errors in this docunemt.</p>

<p>In places the language may resemble <a href="/projects/smith/">SMITH</a>
and <a href="/projects/quylthulg/">Quylthulg</a>, but this was not intended,
and the similarities are purely emergent.</p>

<h2>Program Structure</h2>

<p>A Oozlybub and Murphy program consists of a number of <dfn>variables</dfn> and
a number of objects called <dfn>dynasts</dfn>.  A Oozlybub and Murphy program text
consists of multiple <dfn>parse streams</dfn>.  Each parse stream contains
zero or more variable declarations, and optionally a single dynast.</p>

<h3>Parse Streams</h3>

<p>A parse stream is just a segment, possibly non-contiguous, of the text of
a Oozlybub and Murphy program.  A program starts out with a single parse stream, but
certain <dfn>parse stream manipulation pragmas</dfn> can change this.
These pragmas have the form <code>{@<var>x</var>}</code> and
have a similar syntactic status as comments; they can appear anywhere except
inside a lexeme.</p>

<p>Parse streams are arranged as a ring (a cyclic doubly linked list.)  When parsing
of the program text begins initially, there is already a single pre-created parse stream.
When the program text ends, all parse streams which may be active are deleted.</p>

<p>The meanings of the pragmas are:</p>

<ul>
<li><code>{@+}</code> Create a new parse stream to the right of the current one.</li>
<li><code>{@&gt;}</code> Switch to the parse stream to the right of the current one.</li>
<li><code>{@&lt;}</code> Switch to the parse stream to the left of the current one.</li>
<li><code>{@-}</code> Delete the current parse stream.  The parse stream to the
left of the deleted parse stream will become the new current parse stream.</li>
</ul>

<p>Deleting a parse stream while it contains an unfinished syntactic construct is a syntax
error, just as an end-of-file in that circumstance would be in most other languages.</p>

<p>Providing a concrete example of parse streams in action will be difficult in the
absence of defined syntax for the rest of Oozlybub and Murphy, so we will, for the purposes of
the following demonstration only, pretend that the contents of a parse stream is a sentence of English.
Here is how three parse streams might be managed:</p>

<p><code>The quick {@+}brown{@&gt;}Now is the time{@&lt;}fox{@&lt;} for all good
men to {@+}{@&gt;}Wherefore art thou {@&gt;} jumped over {@&gt;}{@&gt;}Romeo?{@-}
come to the aid of {@&gt;}the lazy dog's tail.{@-}their country.{@-}
</code></p>

<h3>Variables</h3>

<p>All variables are declared in a block at the beginning of a parse stream.
If there is also a dynast in that stream, the variables are private to that dynast;
otherwise they are global and shared by all dynasts.  (<em>Defined in 1.1</em>)
Any dynamically created dynast gets its own private copies of any private
variables the original dynast had; they will initially hold the values they
had in the original, but they are not shared.</p>

<p>The name of a variable in Oozlybub and Murphy is not a fixed, finite-length string of symbols,
as you would find in other programming languages.  No sir!  In Oozlybub and Murphy, each
variable is named by a possibly-infinite set of strings
(over the alphanumeric-plus-spaces alphabet <code>[a-zA-Z0-9 ]</code>),
at least one of which must be infinitely long.  (<em>New in 1.1</em>: spaces
[but no other kinds of whitespace] are allowed in these strings.)</p>

<p>To accomodate this method of identifying a variable, in Oozlybub and Murphy programs,
which are finite, variables are identified using regular expressions
which match their set of names.
An equivalence class of regular expressions is a set of all regular expressions which
accept exactly the same set of strings; each equivalence class of regular expressions
refers to the same, unique Oozlybub and Murphy variable.</p>

<p>(In case you wonder about the implementability of this:
Checking that two regular expressions are
equivalent is decidable: we convert them both to NFAs, then
to DFAs, then minimize those DFAs, then check if the transition
graphs of those DFAs are isomorphic.
Checking that the regular expression accepts at least
one infinitely-long string is also decidable: just look for
a cycle in the DFA's graph.)</p>

<p>Note that these identifier-sets need not be disjoint.  <code>/ma*/</code>
and <code>/mb*/</code> are distinct variables, even though both contain the
string <code>m</code>.  (Note also that we are fudging slightly on how we
consider to have described an infinitely long name; technically we would
want to have a Büchi automaton that specifies an unending repetition with
<sup>ω</sup> instead of *.  But the distinction is subtle enough in this
context that we're gonna let it slide.)</p>

<p>Syntax for giving a variable name is fairly straightforward: it is delimited on
either side by <code>/</code> symbols; the alphanumeric symbols are literals;
textual concatenation is regular expression sequencing,
<code>|</code> is alteration, <code>(</code> and <code>)</code> increase precedence,
and  <code>*</code> is Kleene asteration (zero or more occurrences).
Asteration has higher precedence than sequencing, which has higher precedence
than alteration.  Because none of these operators is alphanumeric nor a space, no
escaping scheme needs to be installed.</p>

<p>Variables are declared with the following syntax (<code>i</code> and <code>a</code>
are the types of the variables, described in the next section):</p>

<pre>
VARIABLES ARE i /pp*/, i /qq*/, a /(0|1)*/.
</pre>

<p>This declares an integer variable identified by the names {<code>p</code>, <code>pp</code>, <code>ppp</code>, ...},
an integer variable identified by the names {<code>q</code>, <code>qq</code>, <code>qqq</code>, ...},
and an array variable identified by the names of all strings of <code>0</code>'s and <code>1</code>'s.</p>

<p>When not in wimpmode (see below), any regular expression which denotes a
variable may not be literally repeated anywhere else in the program.  So in the above example,
it would not be legal to refer to <code>/pp*/</code> further down in the program; an equivalent regular
expression, such as <code>/p|ppp*/</code> or <code>/p*p/</code> or
<code>/pp*|pp*|pp*/</code> would have to be used instead.</p>

<h3>Types</h3>

<p>Oozlybub and Murphy is a statically-typed language, in that variables
as well as values have types, and a value of one type cannot be stored in
a variable of another type.  The types of values, however, are not entirely
disjoint, as we will see, and special considerations may arise for checking
and conversion because of this.</p>

<p>The basic types are:</p>

<ul>
<li><code>i</code>, the type of integers.
<p>These are integers of unbounded extent, both positive and negative.
Literal constants of type <code>i</code> are given in the usual decimal format.
Variables of this type initially contain the value 0.</p>
</li>

<li><code>p</code>, the type of prime numbers.
<p>All prime numbers are integers but not all integers are prime numbers.
Thus, values of prime number type will automatically be coerced to
integers in contexts that require integers; however the reverse is not true,
and in the other direction a conversion function (<code>P?</code>) must
be used.  There are no literal constants of type <code>p</code>.  Variables
of this type initially contain the value 2.</p>
</li>

<li><code>a</code>, the type of arrays of integers.
<p>An integer array has an integer index which is likewise of unbounded
extent, both positive and negative.  Variables of this type initially
contain an empty array value, where all of the entries are 0.</p>
</li>

<li><code>b</code>, the type of booleans.
<p>A boolean has two possible values, <code>true</code> and <code>false</code>.
Note that there are no literal constants of type <code>b</code>; these must be
specified by constructing a tautology or contradiction with boolean (or other) operators.
It is illegal to retrieve the value of a variable of this type before first assigning it,
except to construct a tautology or contradiction.</p>
</li>

<li><code>t</code>, the type of truth-values.
<p>A truth-value has two possible values, <code>yes</code> and <code>no</code>.
There are no literal constants of type <code>t</code>.
It is illegal to retrieve the value of a variable of this type before first assigning it,
except to construct a tautology or contradiction.</p>
</li>

<li><code>z</code>, the type of bits.
<p>A bit has two possible values, <code>one</code> and <code>zero</code>.
There are no literal constants of type <code>z</code>.
It is illegal to retrieve the value of a variable of this type before first assigning it,
except to construct a tautology or contradiction.</p>
</li>

<li><code>c</code>, the type of conditions.
<p>A condition has two possible values, <code>go</code> and <code>nogo</code>.
There are no literal constants of type <code>c</code>.
It is illegal to retrieve the value of a variable of this type before first assigning it,
except to construct a tautology or contradiction.</p>
</li>

</ul>

<h3>Wimpmode</h3>

<p>(<em>New in 1.1</em>) An Oozlybub and Murphy program is in <dfn>wimpmode</dfn> if it
declares a global variable of integer type which matches the string <code>am a wimp</code>,
for example:</p>

<pre>
VARIABLES ARE i /am *a *wimp/.
</pre>

<p>Certain language constructs, noted in this document as such,
are only permissible in wimpmode.
If they are used in a program in which wimpmode is not in effect, a
compile-time error shall occur and the program shall not be executed.</p>

<h3>Dynasts</h3>

<p>Each dynast is labeled with a positive integer and contains an expression.
Only one dynast may be denoted in any given parse stream, but dynasts may also
be created dynamically during program execution.</p>

<p>Program execution begins at the lowest-numbered dynast that exists in the initial program.
When a dynast is executed, the expression of that dynast is evaluated for its side-effects.
If there is a dynast labelled with the next higher integer (i.e. the successor of the label
of the current dynast),
execution continues with that dynast; otherwise, the program halts.
Once a dynast has been executed, it continues to exist until the program
halts, but it may never be executed again.</p>

<p>Evaluation of an expression may have side-effects, including
writing characters to an output channel, reading characters from
an input channel, altering the value of a variable, and creating
a new dynast.</p>

<p>Dynasts are written with the syntax <code>dynast(<var>label</var>) &lt;-&gt; <var>expr</var></code>.  A concrete example follows:</p>

<pre>
dynast(100) &lt;-&gt; for each prime /p*/ below 1000 do write (./p*|p/+1.)
</pre>

<h3>TRIVIA PORTION OF SHOW</h3>

<p>WHO WAS IT FAMOUS MAN THAT SAID THIS?</p>

<ul>
<li>A) RONALD REAGAN</li>
<li>B) RONALD REAGAN</li>
<li>B) RONALD STEWART</li>
<li>C) RENALDO</li>
</ul>

<p>contestant enters lightning round now</p>

<h3>Expressions</h3>

<p>In the following, the letter preceding <var>-expr</var> or <var>-var</var> indicates
the expected type, if any, of that expression or variable.  Where the expressions listed below are infix
expressions, they are listed from highest to lowest precedence.  Unless noted otherwise,
subexpressions are evaluated left to right.</p>

<ul>

<li><code>(.<var>expr</var>.)</code>
<p>Surrounding an expression with <dfn>dotted parens</dfn> gives it that precedence boost
that's just the thing to have it be evaluated before the expression it's in, but
there is a catch.  The number of parens in the dotted parens expression must
match the nesting depth in the following way: if a set of dotted parens is 
nested within <var>n</var> dotted parens, it must contain fib(<var>n</var>)
parens, where fib(<var>n</var>) is the <var>n</var>th member of the
Fibonacci sequence. For example, <code>(.(.0.).)</code> and <code>(.(.((.(((.(((((.0.))))).))).)).).)</code>
are syntactically well-formed expressions (when not nested in any other dotted paren
expression), but <code>(.(((.0.))).)</code> and
<code>(.(.(.0.).).)</code> are not.</p>
</li>

<li><code><var>var</var></code>
<p>A variable evaluates to the value it contains at that point in execution.</p>
</li>

<li><code>0</code>, <code>1</code>, <code>2</code>, <code>3</code>, etc.
<p>Decimal literals evaluate to the expected value of type <code>i</code>.</p>
</li>

<li><code>#myself#</code>
<p>This special nullary token evaluates to the numeric label of the currently
executing dynast.</p> 
</li>

<li><code><var>var</var> := <var>expr</var></code>
<p>Evaluates the <var>expr</var> and stores the result in the specified variable.
The variable and the expression must have the same type.  Evaluates to whatever
<var>expr</var> evaluated to.</p>
</li>

<li><code><var>a-expr</var> [<var>i-expr</var>]</code>
<p>Evaluates to the <code>i</code> stored at the location in the 
array given by <var>i-expr</var>.</p>
</li>

<li><code><var>a-expr</var> [<var>i-expr</var>] := <var>i-expr</var></code>
<p>Evaluates the second <var>i-expr</var> and stores the result in the location
in the array given by the first <var>i-expr</var>.  Evaluates to whatever
the second <var>i-expr</var> evaluated to.</p>
</li>

<li><code><var>a-expr</var> ? <var>i-expr</var></code>
<p>Evaluates to <code>go</code> if <code><var>a-expr</var> [<var>i-expr</var>]</code>
and <code><var>i-expr</var></code> evaluate to the same thing, <code>nogo</code> otherwise.
The <var>i-expr</var> is only evaluated once.</p>
</li>

<li><code>minus <var>i-expr</var></code>
<p>Evaluate to the integer that is zero minus the result of evaluating <var>i-expr</var>.</p>
</li>

<li><code>write <var>i-expr</var></code>
<p>Write the Unicode code point whose number is obtained by evaluating <var>i-expr</var>, to
the standard output channel.  Writing a negative number shall produce one of a number of
amusing and informative messages which are not defined by this document.</p>
</li>

<li><code>#read#</code>
<p>Wait for a Unicode character to become available on the standard
input channel and evaluate to its integer code point value.</p>
</li>

<li><code>not? <var>z-expr</var></code>
<p>Converts a bit value to a boolean value (<code>zero</code> becomes <code>true</code> and <code>one</code> becomes <code>false</code>).</p>
</li>

<li><code>if? <var>b-expr</var></code>
<p>Converts a boolean value to condition value (true becomes go and false becomes nogo).</p>
</li>

<li><code>cvt? <var>c-expr</var></code>
<p>Converts a condition value to a truth-value (<code>go</code> becomes <code>yes</code> and <code>nogo</code> becomes <code>no</code>).</p>
</li>

<li><code>to? <var>t-expr</var></code>
<p>Converts a truth-value to a bit value (<code>yes</code> becomes <code>one</code> and <code>no</code> becomes <code>zero</code>).</p>
</li>

<li><code>P? <var>i-expr</var> [<var>t-var</var>]</code>
<p>If the result of evaluating <var>i-expr</var> is a prime number, evaluates to that
prime number (and has the type <code>p</code>).  If it is not prime, stores the value <code>no</code>
into <var>t-var</var> and evaluates to 2.</p>
</li>

<li><code><var>i-expr</var> * <var>i-expr</var></code>
<p>Evaluates to the product of the two <var>i-expr</var>s.  The result is never
of type <code>p</code>, but the implementation doesn't need to do anything
based on that fact.</p>
</li>

<li><code><var>i-expr</var> + <var>i-expr</var></code>
<p>Evaluates to the sum of the two <var>i-expr</var>s.</p>
</li>

<li><code>exists/dynast <var>i-expr</var></code>
<p>Evaluates to <code>one</code> if a dynast exists with the given label,
or <code>zero</code> if one does not.</p>
</li>

<li><code>copy/dynast <var>i-expr</var>, <var>p-expr</var>, <var>p-expr</var></code>
<p>Creates a new dynast based on an existing one.  The existing one is identified by
the label given in the <var>i-expr</var>.  The new dynast is a copy of the existing
dynast, but with a new label.  The new label is the sum of the two <var>p-expr</var>s.
If a dynast with that label already exists, the program terminates.
(<em>Defined in 1.1</em>) This expression evaluates to the value of the given <var>i-expr</var>.</p>
</li>

<li><code>create/countably/many/dynasts <var>i-expr</var>, <var>i-expr</var></code>
<p>Creates a countably infinite number of dynasts based on an existing one.  The existing one is identified by
the label given in the first <var>i-expr</var>.  The new dynasts are copies of the existing
dynast, but with new labels.  The new labels start at the first odd integer
greater than the second <var>i-expr</var>, and consist of every odd integer greater than that.
If any dynast with such a label already exists, the program terminates.
(<em>Defined in 1.1</em>) This expression evaluates to the value of the first given <var>i-expr</var>.</p>
</li>

<li><code><var>b-expr</var> and <var>b-expr</var></code>
<p>Evaluates to <code>one</code> if both <var>b-expr</var>s are <code>true</code>,
<code>zero</code> otherwise.  Note that this is not short-circuting; both <var>b-expr</var>s
are evaluated.</p>
</li>

<li><code><var>c-expr</var> or <var>c-expr</var></code>
<p>Evaluates to <code>yes</code> if either or both <var>c-expr</var>s are <code>go</code>,
<code>no</code> otherwise.  Note that this is not short-circuting; both <var>c-expr</var>s
are evaluated.</p>
</li>

<li><code>do <var>expr</var></code>
<p>Evaluates the <var>expr</var>, throws away the result, and evaluates to <code>go</code>.</p>
</li>

<li><code><var>c-expr</var> then <var>expr</var></code>
<p><strong>Wimpmode only.</strong>  Evaluates the <var>c-expr</var> on the left-hand side for its side-effects only,
throwing away the result, then evaluates to the result of evaluating the right-hand
side <var>expr</var>.</p>
</li>

<li><code><var>c-expr</var> ,then <var>i-expr</var></code>
<p>(<em>New in 1.1</em>) Evaluates the <var>c-expr</var> on the left-hand side; if it is <code>go</code>,
evaluates to the result of evaluating the right-hand side <var>i-expr</var>;
if it is <code>nogo</code>, evaluates to an unspecified and quite possibly random
integer between 1 and 1000000 inclusive, without evaluating the right-hand side.
Note that this operator has the same precedence as <code>then</code>.</p>
</li>

<li><code>for each prime <var>var</var> below <var>i-expr</var> do <var>i-expr</var></code>
<p>The <var>var</var> must be a declared variable of type <code>p</code>.  The first
<var>i-expr</var> must evaluate to an integer, which we will call <var>k</var>.
The second <var>i-expr</var> is evaluated once for each prime number between 
<var>k</var> and 2, inclusive; each time it is evaluated, <var>var</var> is bound to
a successively smaller prime number between <var>k</var> and 2.
(<em>Defined in 1.1</em>)  Evaluates to the result of the final evaluation of the second <var>i-expr</var>.</p>
</li>

</ul>

<h3>Grammar</h3>

<p>This section attempts to capture and summarize the syntax rules (for a single
parse stream) described above, using an EBNF-like syntax extended with a few
ad-hoc annotations that I don't feel like explaining right now.</p>

<pre>
ParseStream  ::= VarDeclBlock {DynastLit}.
VarDeclBlock ::= "VARIABLES ARE" VarDecl {"," VarDecl} ".".
VarDecl      ::= TypeSpec VarName.
TypeSpec     ::= "i" | "p" | "a" | "b" | "t" | "z" | "c".
VarName      ::= "/" Pattern "/".
Pattern      ::= {[a-zA-Z0-9 ]}
               | Pattern "|" Pattern                    /* ignoring precedence here */
               | Pattern "*"                            /* and here */
               | "(" Pattern ")".
DynastLit    ::= "dynast" "(" Gumber ")" "&lt;-&gt;" Expr.
Expr         ::= Expr1[c] {"then" Expr1 | ",then" Expr1[i]}.
Expr1        ::= Expr2[c] {"or" Expr2[c]}.
Expr2        ::= Expr3[b] {"and" Expr3[b]}.
Expr3        ::= Expr4[i] {"+" Expr4[i]}.
Expr4        ::= Expr5[i] {"*" Expr5[i]}.
Expr5        ::= Expr6[a] {"?" Expr6[i]}.
Expr6        ::= Prim[a] {"[" Expr[i] "]"} [":=" Expr[i]].
Prim         ::= {"("}* "." Expr "." {")"}*             /* remember the Fibonacci rule! */
               | VarName [":=" Expr]
               | Gumber
               | "#myself#"
               | "minus" Expr[i]
               | "write" Expr[i]
               | "#read#"
               | "not?" Expr[z]
               | "if?" Expr[b]
               | "cvt?" Expr[c]
               | "to?" Expr[t]
               | "P?" Expr[i]
               | "exists/dynast" Expr[i]
               | "copy/dynast" Expr[i] "," Expr[p] "," Expr[p]
               | "create/countably/many/dynasts"
                   Expr[i] "," Expr[i]
               | "do" Expr
               | "for" "each" "prime" VarName "below"
                   Expr[i] "do" Expr[i].
Gumber       ::= {[0-9]}.
</pre>

<h3>Boolean Idioms</h3>

<p>Here we show how we can get any value of any of the <code>b</code>, <code>t</code>,
<code>z</code>, and <code>c</code> types, without any constants or variables with
known values of these types.</p>

<pre>
VARIABLES ARE b /b*/.
zero = /b*|b/ and not? to? cvt? if? /b*|b*/
true = not? zero
go = if? true
yes = cvt? go
one = to? yes
false = not? one
nogo = if? false
no = cvt? nogo
</pre>

<h3>Computational Class</h3>

<p>Because the single in-dynast looping construct, <code>for each prime below</code>, is
always a finite loop, the execution of any fixed number of dynasts cannot be Turing-complete.
We must create new dynasts at runtime, and continue execution in them, if we want
any chance at being Turing-complete.  We demonstrate this by showing an example of
a (conjecturally) infinite loop in Oozlybub and Murphy, an idiom which will doubtless
come in handy in real programs.</p>

<pre>
VARIABLES ARE p /p*/, p /q*/.
dynast(3) &lt;-&gt;
  (. do (. if? not? exists/dynast 5 ,then
       create/countably/many/dynasts #myself#, 5 .) .) ,then
  (. for each prime /p*|p/ below #myself#+2 do
       for each prime /q*|q/ below /p*|pp/+1 do
         if? not? exists/dynast /p*|p|p/+/q*|q|q/ ,then
           copy/dynast #myself#, /p*|ppp/, /q*|qqq/ .)
</pre>

<p>As you can see, the ability to loop indefinitely in Oozlybub and Murphy hinges on whether
Goldbach's Conjecture is true or not.  Looping forever requires creating an unbounded number of
new dynasts.  We can create all the odd-numbered dynasts at once, but that won't be enough
to loop forever, as we must proceed to the next highest numbered dynast after executing a
dynast.  So we must create new dynasts with successively higher even integer labels, and these
can only be created by summing two primes.  So, if Goldbach's conjecture is false, then there
is some even number greater than two which is not the sum of two primes; thus there is some
dynast that cannot be created by a running Oozlybub and Murphy program, thus it is not possible
to loop forever in Oozlybub and Murphy, thus Oozlybub and Murphy is not Turing-complete
(because it cannot simulate any Turing machine that loops forever.)</p>

<p>It should not however be difficult to show that Oozlybub and Murphy is Turing-complete under
the assumption that Goldbach's Conjecture is true.  If Goldbach's Conjecture is true, then the above program
is an infinite loop.  We need only add to it appropriate conditional instructions to,
say, simulate the execution of an arbitrarily-chosen Turing machine.  An array can serve as the
tape, and an integer can serve as the head.  Another integer can serve as the state of the finite
control.  The integer can be tested against various fixed integers by establishing an array for each
of these fixed integers and using the <code>?</code> operator against each in turn; each branch
can mutate the tape, tape head, and finite control as desired.  The program can halt by neglecting
to create a new even dynast to execute next, or by trying to create a dynast with a label that
already exists.</p>

<p>Happy FLIMPING,
<br/>Chris Pressey
<br/>December 1, 2010
<br/>Evanston, Illinois, USA
</p>

</body>
</html>
