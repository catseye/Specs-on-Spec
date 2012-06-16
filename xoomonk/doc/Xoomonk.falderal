-> encoding: UTF-8

The Xoomonk Programming Language
================================

Language version 0.1 Pretty Much Complete But Maybe Not Totally Finalized

Abstract
--------

_Xoomonk_ is a programming language in which _malingering updatable stores_
are first-class objects.  Malingering updatable stores unify several language
constructs, including procedure activations, named parameters, and object-like
data structures.

Description
-----------

Overall, Xoomonk looks like a "typical" imperative language.  The result
of evaluating an expression can be assigned to a variable, and the contents
of a variable can be used in a further expression.

| a := 1
| b := a
| print b
= 1

However, blocks of these imperative statements can appear as terms in
expressions; such blocks evaluate to entire updatable stores.

| a := {
|   c := 5
|   d := c
| }
| print a
= [c=5,d=5]

Once created, a store can be updated and accessed.

| a := {
|   c := 5
|   d := c
| }
| print a
| a.d := 7
| print a
| print a.c
= [c=5,d=5]
= [c=5,d=7]
= 5

A store can also be assigned to a variable after creation.  Stores are
accessed by reference, so this creates two aliases to the same store.

| a := {
|   c := 5
|   d := c
| }
| b := a
| b.c := 17
| print a
| print b
= [c=17,d=5]
= [c=17,d=5]

To create an independent copy of the store, the postfix `*` operator is
used.

| a := {
|   c := 5
|   d := c
| }
| b := a*
| b.c := 17
| print a
| print b
= [c=5,d=5]
= [c=17,d=5]

Empty blocks are permissible.

| a := {}
| print a
= []

Once a store has been created, only those variables defined in the store
can be updated and accessed -- new variables cannot be added.

| a := { b := 6 }
| print a.c
? Attempt to access an undefined variable

| a := { b := 6 }
| a.c := 12
? Attempt to assign an undefined variable

Stores and integers are the only two data types in Xoomonk.  However, there
are some special forms of the print statement, demonstrated here, which
allow for textual output.

| a := 65
| print char a
| print string "Hello, world!"
| print string "The value of a is ";
| print a;
| print "!"
= A
= Hello, world!
= The value of a is 65!

Xoomonk enforces strict block scope.  Variables can be shadowed in an
inner block.

| a := 14
| b := {
|   a := 12
|   print a
| }
| print a
= 12
= 14

The special identifier `^` refers to the lexically enclosing store, that is,
the store which was in effect when the this store was defined.

| a := 14
| b := {
|   a := 12
|   print ^.a
| }
| print b.a
= 14
= 12

As long as a variable is assigned somewhere in a block, it can be accessed
before it is first assigned.  In this case, it will hold the integer value 0.
It need not stay an integer, for typing is dynamic.

| b := {
|   print a
|   a := 12
|   print a
|   a := { c := 13 }
|   print a
| }
| print b
= 0
= 12
= [c=13]
= [a=[c=13]]

We now present today's main feature.

It's important to understand that a block need not define all the
variables used in it.  Such blocks do not immediately evaluate to a store.
Instead, they evaluate to an object called an _unsaturated store_.

Or, to put it another way:

If, in a block, you refer to a variable which has not yet been given
a value in that updatable store, the computations within the block are not
performed until that variable is given a value.  Such a store is called an
_unsaturated store_.

| a := {
|   d := c
| }
| print a
= [c=?,d=*]

An unsaturated store behaves similarly to a saturated store in certain
respects.  In particular, unsaturated stores can be updated.  If doing
so means that all of the undefined variables in the store are now defined,
the block associated with that store is evaluated, and the store becomes
saturated.  In this sense, an unsaturated store is like a promise, and
this bears some resemblance to lazy evaluation (thus the term _malingering_).

| a := {
|   print string "executing block"
|   d := c
| }
| print a
| a.c := 7
| print a
= [c=?,d=*]
= executing block
= [c=7,d=7]

Once a store has become saturated, the block associated with it is not
executed again.

| a := {
|   d := c
| }
| a.c := 7
| print a
| a.c := 4
| print a
= [c=7,d=7]
= [c=4,d=7]

The main program is a block.  If it is unsaturated, no execution occurs
at all.

| print "hello"
| a := 14
| b := c
= 

Variables cannot generally be accessed from an unsaturated store.

| a := {
|   d := c
| }
| x := a.c
? Attempt to access an unassigned variable

| a := {
|   d := c
| }
| x := a.d
? Attempt to access an unresolved variable

This is true, even if the variable is assigned a constant expression
inside the block.

| a := {
|   b := 7
|   d := c
| }
| x := a.b
? Attempt to access an unresolved variable

If, however, the unsaturated store contains some variables that have
been updated since the store was created, those variable may be accessed.

| a := {
|   print "executing block"
|   p := q
|   d := c
| }
| a.q = 7
| print a.q
= 7

Nor is it possible to assign a variable in an unsaturated store which is
defined somewhere in the block.

| a := {
|   b := 7
|   d := c
| }
| a.b := 4
? Attempt to assign an unresolved variable

A variable is considered unresolved even if it is assigned within the block,
if that assignment takes place during or after its first use in an expresion.

| a := {
|   b := b
| }
| a.b := 5
| print a
= [b=5]

| a := {
|   print string "executing block"
|   l := b
|   b := 3
|   l := 3
| }
| print string "saturating store"
| a.b := 5
| print a
= saturating store
= executing block
= [b=3,l=3]

We now describe how this language is (we reasonably assume) Turing-complete.

Operations are accomplished with certain built-in unsaturated stores.  For
example, there is a store called `add`, which can be used for addition.  These
built-in stores are globally available; they do not exist in any particular
store themselves.  One uses the `$` prefix operator to access this global
namespace.

| print $add
| $add.x := 3
| $add.y := 5
| print $add.result
| print $add
= [x=?,y=?,result=*]
= 8
= [x=3,y=5,result=8]

Because using a built-in operation store in this way saturates it, it cannot
be used again.  Typically you want to make a copy of the store first, and use
that, leaving the built-in store unmodified.

| o1 := $add*
| o1.x := 4
| o1.y := 7
| o2 := $add*
| o2.x := o1.result
| o2.y := 9
| print o2.result
= 20

Since Xoomonk is not a strictly minimalist language, there is a selection
of built-in stores which provide useful operations: `$add`, `$sub`, `$mul`,
`$div`, `$gt`, and `$not`.

Decision-making is also accomplished with a built-in store, `if`.  This store
contains variables caled `cond`, `then`, and `else`.  `cond` should
be an integer, and `then` and `else` should be unsaturated stores where `x` is
unassigned.  When the first three are assigned values, if `cond` is nonzero,
it is assigned to `x` in the `then` store; otherwise, if it is zero, it is
assigned to `x` in the `else` store.

| o1 := $if*
| o1.then := {
|   y := x
|   print "condition is true"
| }
| o1.else := {
|   y := x
|   print "condition is false"
| }
| o1.cond := 0
= condition is false

| o1 := $if*
| o1.then := {
|   y := x
|   print "condition is true"
| }
| o1.else := {
|   y := x
|   print "condition is false"
| }
| o1.cond := 1
= condition is true

Repetition is also accomplished with a built-in store, `loop`.  This store
contains an unassigned variable called `do`.  When it is assigned a value,
assumed to be an unsaturated store, a copy of it is made.  The variable
`x` inside that copy is assigned the value 0.  This is supposed to saturate
the store.  The variable `continue` is then accessed from the store.  If
it is nonzero, the process repeats, with another copy of the `do` store
getting 0 assigned to its `x`, and so forth.

| l := $loop*
| counter := 5
| l.do := {
|   y := x
|   print ^.counter
|   o := $sub*
|   o.x := ^.counter
|   o.y := 1
|   ^.counter := o.result
|   continue := o.result
| }
| print string "done!"
= 5
= 4
= 5
= 2
= 1
= done!

Because the `loop` construct will always execute the `do` store at least once
(even assuming its only unassigned variable is `x`), it acts like a so-called
`repeat` loop.  It can be used in conjunction with `if` to simulate a
so-called `while` loop.  With this loop, the built-in operations provided,
and variables which may contain unbounded integer values, Xoomonk should
be uncontroversially Turing-complete.

Finally, there is no provision for defining functions or procedures, because
malingering stores can act as these constructs.

| perimeter := {
|   o1 := $mul*
|   o1.x := x
|   o1.y := 2
|   o2 := $mul*
|   o2.x := y
|   o2.y := 2
|   o3 := $add*
|   o3.x := o1.result
|   o3.y := o2.result
|   result := o3.result
| }
| p1 := perimeter*
| p1.x := 13
| p1.y := 6
| print p1.result
| p2 := perimeter*
| p2.x := 4
| p2.y := 1
| print p2.result
= 38
= 10

Grammar
-------

    Xoomonk ::= { Stmt }.
    Stmt    ::= Assign | Print.
    Assign  ::= Ref ":=" Expr.
    Print   ::= "print" ("string" <string> | "char" Expr | Expr) [";"].
    Expr    ::= (Block | Ref | Const) ["*"].
    Block   ::= "{" { Stmt } "}".
    Ref     ::= Name {"." Name}.
    Name    ::= "^" | "$" <alphanumeric> | <alphanumeric>.
    Const   ::= <integer-literal>.

Discussion
----------

There is some similarity with Wouter van Oortmerssen's language Bla, in that
function environments are very close cousins of updatable stores.  But
Xoomonk, quite unlike Bla, is an imperative language; once created, a store
may be updated at any point.  And, of course, this property is exploited in
the introduction of malingering stores.

Xoomonk originally had an infix operator `&`, which takes two stores as its
arguments, at least one of which must be saturated, and evaluates to a third
store which is the result of merging the two argument stores.  This result store
may be saturated even if only one of the argument stores was saturated, if
the saturated store gave all the values that the unsaturated store needed.
This operator was dropped because it is mostly syntactic sugar for assigning
each of the desired variables from one store to the other.  However, it does
admittedly provide a very natural syntax, which orthogonally includes "named
arguments", when using unsaturated stores as procedures:

    perimeter = {
      # see example above
    }
    g := perimeter* & { x := 13 y := 6 }
    print g.result

Xoomonk, as a project, is also an experiment in _test-driven language
design_.  As you can see, I've described the language with a series of
examples, written in (something close to) Falderal format, each of which
could be used as a test case.  This should make implementing an interpreter
or compiler for the language much easier, when I get around to that.

Happy malingering!

-Chris Pressey  
Cat's Eye Technologies  
August 7, 2011  
Evanston, IL
