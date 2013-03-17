Oozlybub and Murphy
===================

Language version 1.1

Overview
--------

This document describes a new programming language. The name of this
language is Oozlybub and Murphy. Despite appearances, this name refers
to a single language. The majority of the language is named Oozlybub.
The fact that the language is not entirely named Oozlybub is named
Murphy. Deal with it.

For the sake of providing an "olde tyme esoterickal de-sign", the
language combines several unusual features, including multiple
interleaved parse streams, infinitely long variable names, gratuitously
strong typing, and only-conjectural Turing completeness. While no
implementation of the language exists as of this writing, it is thought
to be sufficiently consistent to be implementable, modulo any errors in
this docunemt.

In places the language may resemble [SMITH][] and [Quylthulg][], but
this was not intended, and the similarities are purely emergent.

[SMITH]: http://catseye.tc/node/SMITH.html
[Quylthulg]: http://catseye.tc/node/Quylthulg.html

Program Structure
-----------------

A Oozlybub and Murphy program consists of a number of variables and a
number of objects called _dynasts_. A Oozlybub and Murphy program text
consists of multiple parse streams. Each parse stream contains zero or
more variable declarations, and optionally a single dynast.

### Parse Streams

A parse stream is just a segment, possibly non-contiguous, of the text
of a Oozlybub and Murphy program. A program starts out with a single
parse stream, but certain parse stream manipulation pragmas can change
this. These pragmas have the form `{@x}` and have a similar syntactic
status as comments; they can appear anywhere except inside a lexeme.

Parse streams are arranged as a ring (a cyclic doubly linked list.) When
parsing of the program text begins initially, there is already a single
pre-created parse stream. When the program text ends, all parse streams
which may be active are deleted.

The meanings of the pragmas are:

-   `{@+}` Create a new parse stream to the right of the current one.
-   `{@>}` Switch to the parse stream to the right of the current one.
-   `{@<}` Switch to the parse stream to the left of the current one.
-   `{@-}` Delete the current parse stream. The parse stream to the left
    of the deleted parse stream will become the new current parse
    stream.

Deleting a parse stream while it contains an unfinished syntactic
construct is a syntax error, just as an end-of-file in that circumstance
would be in most other languages.

Providing a concrete example of parse streams in action will be
difficult in the absence of defined syntax for the rest of Oozlybub and
Murphy, so we will, for the purposes of the following demonstration
only, pretend that the contents of a parse stream is a sentence of
English. Here is how three parse streams might be managed:

`The quick {@+}brown{@>}Now is the time{@<}fox{@<} for all good men to {@+}{@>}Wherefore art thou {@>} jumped over {@>}{@>}Romeo?{@-} come to the aid of {@>}the lazy dog's tail.{@-}their country.{@-}`

### Variables

All variables are declared in a block at the beginning of a parse
stream. If there is also a dynast in that stream, the variables are
private to that dynast; otherwise they are global and shared by all
dynasts. (*Defined in 1.1*) Any dynamically created dynast gets its own
private copies of any private variables the original dynast had; they
will initially hold the values they had in the original, but they are
not shared.

The name of a variable in Oozlybub and Murphy is not a fixed,
finite-length string of symbols, as you would find in other programming
languages. No sir! In Oozlybub and Murphy, each variable is named by a
possibly-infinite set of strings (over the alphanumeric-plus-spaces
alphabet `[a-zA-Z0-9 ]`), at least one of which must be infinitely long.
(*New in 1.1*: spaces [but no other kinds of whitespace] are allowed in
these strings.)

To accomodate this method of identifying a variable, in Oozlybub and
Murphy programs, which are finite, variables are identified using
regular expressions which match their set of names. An equivalence class
of regular expressions is a set of all regular expressions which accept
exactly the same set of strings; each equivalence class of regular
expressions refers to the same, unique Oozlybub and Murphy variable.

(In case you wonder about the implementability of this: Checking that
two regular expressions are equivalent is decidable: we convert them
both to NFAs, then to DFAs, then minimize those DFAs, then check if the
transition graphs of those DFAs are isomorphic. Checking that the
regular expression accepts at least one infinitely-long string is also
decidable: just look for a cycle in the DFA's graph.)

Note that these identifier-sets need not be disjoint. `/ma*/` and
`/mb*/` are distinct variables, even though both contain the string `m`.
(Note also that we are fudging slightly on how we consider to have
described an infinitely long name; technically we would want to have a
Büchi automaton that specifies an unending repetition with ^ω^ instead
of \*. But the distinction is subtle enough in this context that we're
gonna let it slide.)

Syntax for giving a variable name is fairly straightforward: it is
delimited on either side by `/` symbols; the alphanumeric symbols are
literals; textual concatenation is regular expression sequencing, `|` is
alteration, `(` and `)` increase precedence, and `*` is Kleene
asteration (zero or more occurrences). Asteration has higher precedence
than sequencing, which has higher precedence than alteration. Because
none of these operators is alphanumeric nor a space, no escaping scheme
needs to be installed.

Variables are declared with the following syntax (`i` and `a` are the
types of the variables, described in the next section):

    VARIABLES ARE i /pp*/, i /qq*/, a /(0|1)*/.

This declares an integer variable identified by the names {`p`, `pp`,
`ppp`, ...}, an integer variable identified by the names {`q`, `qq`,
`qqq`, ...}, and an array variable identified by the names of all
strings of `0`'s and `1`'s.

When not in wimpmode (see below), any regular expression which denotes a
variable may not be literally repeated anywhere else in the program. So
in the above example, it would not be legal to refer to `/pp*/` further
down in the program; an equivalent regular expression, such as
`/p|ppp*/` or `/p*p/` or `/pp*|pp*|pp*/` would have to be used instead.

### Types

Oozlybub and Murphy is a statically-typed language, in that variables as
well as values have types, and a value of one type cannot be stored in a
variable of another type. The types of values, however, are not entirely
disjoint, as we will see, and special considerations may arise for
checking and conversion because of this.

The basic types are:

-   `i`, the type of integers.

    These are integers of unbounded extent, both positive and negative.
    Literal constants of type `i` are given in the usual decimal format.
    Variables of this type initially contain the value 0.

-   `p`, the type of prime numbers.

    All prime numbers are integers but not all integers are prime
    numbers. Thus, values of prime number type will automatically be
    coerced to integers in contexts that require integers; however the
    reverse is not true, and in the other direction a conversion
    function (`P?`) must be used. There are no literal constants of type
    `p`. Variables of this type initially contain the value 2.

-   `a`, the type of arrays of integers.

    An integer array has an integer index which is likewise of unbounded
    extent, both positive and negative. Variables of this type initially
    contain an empty array value, where all of the entries are 0.

-   `b`, the type of booleans.

    A boolean has two possible values, `true` and `false`. Note that
    there are no literal constants of type `b`; these must be specified
    by constructing a tautology or contradiction with boolean (or other)
    operators. It is illegal to retrieve the value of a variable of this
    type before first assigning it, except to construct a tautology or
    contradiction.

-   `t`, the type of truth-values.

    A truth-value has two possible values, `yes` and `no`. There are no
    literal constants of type `t`. It is illegal to retrieve the value
    of a variable of this type before first assigning it, except to
    construct a tautology or contradiction.

-   `z`, the type of bits.

    A bit has two possible values, `one` and `zero`. There are no
    literal constants of type `z`. It is illegal to retrieve the value
    of a variable of this type before first assigning it, except to
    construct a tautology or contradiction.

-   `c`, the type of conditions.

    A condition has two possible values, `go` and `nogo`. There are no
    literal constants of type `c`. It is illegal to retrieve the value
    of a variable of this type before first assigning it, except to
    construct a tautology or contradiction.

### Wimpmode

(*New in 1.1*) An Oozlybub and Murphy program is in wimpmode if it
declares a global variable of integer type which matches the string
`am a wimp`, for example:

    VARIABLES ARE i /am *a *wimp/.

Certain language constructs, noted in this document as such, are only
permissible in wimpmode. If they are used in a program in which wimpmode
is not in effect, a compile-time error shall occur and the program shall
not be executed.

### Dynasts

Each dynast is labeled with a positive integer and contains an
expression. Only one dynast may be denoted in any given parse stream,
but dynasts may also be created dynamically during program execution.

Program execution begins at the lowest-numbered dynast that exists in
the initial program. When a dynast is executed, the expression of that
dynast is evaluated for its side-effects. If there is a dynast labelled
with the next higher integer (i.e. the successor of the label of the
current dynast), execution continues with that dynast; otherwise, the
program halts. Once a dynast has been executed, it continues to exist
until the program halts, but it may never be executed again.

Evaluation of an expression may have side-effects, including writing
characters to an output channel, reading characters from an input
channel, altering the value of a variable, and creating a new dynast.

Dynasts are written with the syntax `dynast(label) <-> expr`. A concrete
example follows:

    dynast(100) <-> for each prime /p*/ below 1000 do write (./p*|p/+1.)

### TRIVIA PORTION OF SHOW

WHO WAS IT FAMOUS MAN THAT SAID THIS?

-   A) RONALD REAGAN
-   B) RONALD REAGAN
-   B) RONALD STEWART
-   C) RENALDO

contestant enters lightning round now

### Expressions

In the following, the letter preceding -expr or -var indicates the
expected type, if any, of that expression or variable. Where the
expressions listed below are infix expressions, they are listed from
highest to lowest precedence. Unless noted otherwise, subexpressions are
evaluated left to right.

-   `(.expr.)`

    Surrounding an expression with dotted parens gives it that
    precedence boost that's just the thing to have it be evaluated
    before the expression it's in, but there is a catch. The number of
    parens in the dotted parens expression must match the nesting depth
    in the following way: if a set of dotted parens is nested within n
    dotted parens, it must contain fib(n) parens, where fib(n) is the
    nth member of the Fibonacci sequence. For example, `(.(.0.).)` and
    `(.(.((.(((.(((((.0.))))).))).)).).)` are syntactically well-formed
    expressions (when not nested in any other dotted paren expression),
    but `(.(((.0.))).)` and `(.(.(.0.).).)` are not.

-   `var`

    A variable evaluates to the value it contains at that point in
    execution.

-   `0`, `1`, `2`, `3`, etc.

    Decimal literals evaluate to the expected value of type `i`.

-   `#myself#`

    This special nullary token evaluates to the numeric label of the
    currently executing dynast.

-   `var := expr`

    Evaluates the expr and stores the result in the specified variable.
    The variable and the expression must have the same type. Evaluates
    to whatever expr evaluated to.

-   `a-expr [i-expr]`

    Evaluates to the `i` stored at the location in the array given by
    i-expr.

-   `a-expr [i-expr] := i-expr`

    Evaluates the second i-expr and stores the result in the location in
    the array given by the first i-expr. Evaluates to whatever the
    second i-expr evaluated to.

-   `a-expr ? i-expr`

    Evaluates to `go` if `a-expr [i-expr]` and `i-expr` evaluate to the
    same thing, `nogo` otherwise. The i-expr is only evaluated once.

-   `minus i-expr`

    Evaluate to the integer that is zero minus the result of evaluating
    i-expr.

-   `write i-expr`

    Write the Unicode code point whose number is obtained by evaluating
    i-expr, to the standard output channel. Writing a negative number
    shall produce one of a number of amusing and informative messages
    which are not defined by this document.

-   `#read#`

    Wait for a Unicode character to become available on the standard
    input channel and evaluate to its integer code point value.

-   `not? z-expr`

    Converts a bit value to a boolean value (`zero` becomes `true` and
    `one` becomes `false`).

-   `if? b-expr`

    Converts a boolean value to condition value (true becomes go and
    false becomes nogo).

-   `cvt? c-expr`

    Converts a condition value to a truth-value (`go` becomes `yes` and
    `nogo` becomes `no`).

-   `to? t-expr`

    Converts a truth-value to a bit value (`yes` becomes `one` and `no`
    becomes `zero`).

-   `P? i-expr [t-var]`

    If the result of evaluating i-expr is a prime number, evaluates to
    that prime number (and has the type `p`). If it is not prime, stores
    the value `no` into t-var and evaluates to 2.

-   `i-expr * i-expr`

    Evaluates to the product of the two i-exprs. The result is never of
    type `p`, but the implementation doesn't need to do anything based
    on that fact.

-   `i-expr + i-expr`

    Evaluates to the sum of the two i-exprs.

-   `exists/dynast i-expr`

    Evaluates to `one` if a dynast exists with the given label, or
    `zero` if one does not.

-   `copy/dynast i-expr, p-expr, p-expr`

    Creates a new dynast based on an existing one. The existing one is
    identified by the label given in the i-expr. The new dynast is a
    copy of the existing dynast, but with a new label. The new label is
    the sum of the two p-exprs. If a dynast with that label already
    exists, the program terminates. (*Defined in 1.1*) This expression
    evaluates to the value of the given i-expr.

-   `create/countably/many/dynasts i-expr, i-expr`

    Creates a countably infinite number of dynasts based on an existing
    one. The existing one is identified by the label given in the first
    i-expr. The new dynasts are copies of the existing dynast, but with
    new labels. The new labels start at the first odd integer greater
    than the second i-expr, and consist of every odd integer greater
    than that. If any dynast with such a label already exists, the
    program terminates. (*Defined in 1.1*) This expression evaluates to
    the value of the first given i-expr.

-   `b-expr and b-expr`

    Evaluates to `one` if both b-exprs are `true`, `zero` otherwise.
    Note that this is not short-circuting; both b-exprs are evaluated.

-   `c-expr or c-expr`

    Evaluates to `yes` if either or both c-exprs are `go`, `no`
    otherwise. Note that this is not short-circuting; both c-exprs are
    evaluated.

-   `do expr`

    Evaluates the expr, throws away the result, and evaluates to `go`.

-   `c-expr then expr`

    **Wimpmode only.** Evaluates the c-expr on the left-hand side for
    its side-effects only, throwing away the result, then evaluates to
    the result of evaluating the right-hand side expr.

-   `c-expr ,then i-expr`

    (*New in 1.1*) Evaluates the c-expr on the left-hand side; if it is
    `go`, evaluates to the result of evaluating the right-hand side
    i-expr; if it is `nogo`, evaluates to an unspecified and quite
    possibly random integer between 1 and 1000000 inclusive, without
    evaluating the right-hand side. Note that this operator has the same
    precedence as `then`.

-   `for each prime var below i-expr do i-expr`

    The var must be a declared variable of type `p`. The first i-expr
    must evaluate to an integer, which we will call k. The second i-expr
    is evaluated once for each prime number between k and 2, inclusive;
    each time it is evaluated, var is bound to a successively smaller
    prime number between k and 2. (*Defined in 1.1*) Evaluates to the
    result of the final evaluation of the second i-expr.

### Grammar

This section attempts to capture and summarize the syntax rules (for a
single parse stream) described above, using an EBNF-like syntax extended
with a few ad-hoc annotations that I don't feel like explaining right
now.

    ParseStream  ::= VarDeclBlock {DynastLit}.
    VarDeclBlock ::= "VARIABLES ARE" VarDecl {"," VarDecl} ".".
    VarDecl      ::= TypeSpec VarName.
    TypeSpec     ::= "i" | "p" | "a" | "b" | "t" | "z" | "c".
    VarName      ::= "/" Pattern "/".
    Pattern      ::= {[a-zA-Z0-9 ]}
                   | Pattern "|" Pattern                    /* ignoring precedence here */
                   | Pattern "*"                            /* and here */
                   | "(" Pattern ")".
    DynastLit    ::= "dynast" "(" Gumber ")" "<->" Expr.
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

### Boolean Idioms

Here we show how we can get any value of any of the `b`, `t`, `z`, and
`c` types, without any constants or variables with known values of these
types.

    VARIABLES ARE b /b*/.
    zero = /b*|b/ and not? to? cvt? if? /b*|b*/
    true = not? zero
    go = if? true
    yes = cvt? go
    one = to? yes
    false = not? one
    nogo = if? false
    no = cvt? nogo

### Computational Class

Because the single in-dynast looping construct, `for each prime below`,
is always a finite loop, the execution of any fixed number of dynasts
cannot be Turing-complete. We must create new dynasts at runtime, and
continue execution in them, if we want any chance at being
Turing-complete. We demonstrate this by showing an example of a
(conjecturally) infinite loop in Oozlybub and Murphy, an idiom which
will doubtless come in handy in real programs.

    VARIABLES ARE p /p*/, p /q*/.
    dynast(3) <->
      (. do (. if? not? exists/dynast 5 ,then
           create/countably/many/dynasts #myself#, 5 .) .) ,then
      (. for each prime /p*|p/ below #myself#+2 do
           for each prime /q*|q/ below /p*|pp/+1 do
             if? not? exists/dynast /p*|p|p/+/q*|q|q/ ,then
               copy/dynast #myself#, /p*|ppp/, /q*|qqq/ .)

As you can see, the ability to loop indefinitely in Oozlybub and Murphy
hinges on whether Goldbach's Conjecture is true or not. Looping forever
requires creating an unbounded number of new dynasts. We can create all
the odd-numbered dynasts at once, but that won't be enough to loop
forever, as we must proceed to the next highest numbered dynast after
executing a dynast. So we must create new dynasts with successively
higher even integer labels, and these can only be created by summing two
primes. So, if Goldbach's conjecture is false, then there is some even
number greater than two which is not the sum of two primes; thus there
is some dynast that cannot be created by a running Oozlybub and Murphy
program, thus it is not possible to loop forever in Oozlybub and Murphy,
thus Oozlybub and Murphy is not Turing-complete (because it cannot
simulate any Turing machine that loops forever.)

It should not however be difficult to show that Oozlybub and Murphy is
Turing-complete under the assumption that Goldbach's Conjecture is true.
If Goldbach's Conjecture is true, then the above program is an infinite
loop. We need only add to it appropriate conditional instructions to,
say, simulate the execution of an arbitrarily-chosen Turing machine. An
array can serve as the tape, and an integer can serve as the head.
Another integer can serve as the state of the finite control. The
integer can be tested against various fixed integers by establishing an
array for each of these fixed integers and using the `?` operator
against each in turn; each branch can mutate the tape, tape head, and
finite control as desired. The program can halt by neglecting to create
a new even dynast to execute next, or by trying to create a dynast with
a label that already exists.

Happy FLIMPING,  
Chris Pressey  
December 1, 2010  
Evanston, Illinois, USA
