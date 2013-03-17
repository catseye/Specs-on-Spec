Madison
=======

Version 0.1
December 2011, Chris Pressey, Cat's Eye Technologies

Abstract
--------

Madison is a language in which one can state proofs of properties
of term-rewriting systems.  Classical methods of automated reasoning,
such as resolution, are not used; indeed, term-rewriting itself is
used to check the proofs.  Both direct proof and proof by induction
are supported.  Induction in a proof must be across a structure which
has a well-founded inductive definition.  Such structures can be
thought of as types, although this is largely nominal; the traditional
typelessness of term-rewiting systems is largely retained.

Term-rewriting
--------------

Madison has at its core a simple term-rewriting language.  It is of
a common form which should be unsurprising to anyone who has worked
at all with term rewriting.  A typical simple program contains
a set of rules and a term on which to apply those rules.  Each
rule is a pair of terms; either term of the pair may contain
variables, but any variable that appears on the r.h.s must also
appear on the l.h.s.  A rule matches a term if is the same as
the term with the exception of the variables, which are bound
to its subterms; applying a matching rule replaces the term
with the r.h.s. of the rule, with the variables expanded approp-
riately.  Rules are applied in an innermost, leftmost fashion to
the term, corresponding to eager evaluation.  Rewriting terminates
when there is no rule whose l.h.s. matches the current incarnation
of the term being rewritten.

A term is either an atom, which is a symbol that stands alone,
or a constructor, which is a symbol followed by a comma-separated list
of subterms enclosed in parentheses.  Symbols may consist of letters,
digits, and hyphens, with no intervening whitespace.  A symbol is
a variable symbol if it begins with a capital letter.  Variable
symbols may also begin with underscores, but these may only occur
in the l.h.s. of a rewrite rule, to indicate that we don't care
what value is bound to the variable and we won't be using it on
the r.h.s.

(The way we are using the term "constructor" may be slightly non-
standard; in some other sources, this is called a "function symbol",
and a "constructor" is a subtly different thing.)

Because the rewriting language is merely a component (albeit the
core component) of a larger system, the aforementioned typical
simple program must be cast into some buttressing syntax.  A full
program consists of a `let` block which contains the rules
and a `rewrite` admonition which specifies the term to be re-
written.  An example follows.

| let
|   leftmost(tree(X,Y)) -> leftmost(X)
|   leftmost(leaf(X))   -> X
| in
|   rewrite leftmost(tree(tree(leaf(alice),leaf(grace)),leaf(dan)))
= alice

In the above example, there are two rules for the constructor
`leftmost/1`.  The first is applied to the outer tree to obtain
a new leftmost constructor containing the inner tree; the first
is applied again to obtain a new leftmost constructor containing
the leaf containing `alice`; and the second is applied to that
leaf term to obtain just `alice`.  At that point, no more rules
apply, so rewriting terminates, yielding `alice`.

Madison is deterministic; if rules overlap, the first one given
(syntactically) is used.  For this reason, it is a good idea
to order rules from most specific to least specific.

I used the phrase "typical simple program" above because I was
trying intentionally to avoid saying "simplest program".  In fact,
technically no `let` block is required, so you can write some
really trivial Madison programs, like the following:

| rewrite cat
= cat

I think that just about covers the core term-rewriting language.
Term-rewriting is Turing-complete, so Madison is too.  If you
wish to learn more about term rewriting, there are several good
books and webpages on the subject; I won't go into it further
here.

Proof-Checking
--------------

My desire with Madison was to design a language in which you
can prove things.  Not a full-blown theorem prover -- just a
proof checker, where you supply a proof and it confirms either
that the proof holds or doesn't hold.  (Every theorem prover
has at its core a proof checker, but it comes bundled with a lot of
extra machinery to search the space of possible proofs cleverly,
looking for one which will pass the proof-checking phase.)

It's no coincidence that Madison is built on top of a term-rewriting
language.  For starters, a proof is very similar to the execution
trace of a term being rewritten.  In each of the steps of the proof,
the statement to be proved is transformed by replacing some part
of it with some equally true thing -- in other words, rewritten.
In fact, Post Canonical Systems were an early kind of rewriting
system, devised by Emil Post to (as I understand it) illustrate this
similarity, and to show that proofs could be mechanically carried out
in a rewriting system.

So: given a term-rewriting language, we can give a trivial kind
of proof simply by stating the rewrite steps that *should* occur
when a term is rewritten, and check that proof by rewriting the term
and confirming that those were in fact the steps that occurred.

For the purpose of stating these sequences of rewrite steps to be
checked, Madison has a `theorem..proof..qed` form.  To demonstrate
this form, let's use Madison to prove that 2 + 2 = 4, using Peano
arithmetic.

| let
|   add(s(X),Y) -> add(X,s(Y))
|   add(z,Y)    -> Y
| in theorem
|   add(s(s(z)),s(s(z))) ~> s(s(s(s(z))))
| proof
|   add(s(s(z)),s(s(z)))
|   -> add(s(z),s(s(s(z)))) [by add.1]
|   -> add(z,s(s(s(s(z))))) [by add.1]
|   -> s(s(s(s(z))))        [by add.2]
| qed
= true

The basic syntax should be fairly apparent.  The `theorem` block
contains the statement to be proved.  The `~>` means "rewrites
in zero or more steps to".  So, here, we are saying that 2 + 2
(in Peano notation) rewrites, in zero or more steps, to 4.

The `proof` block contains the actual series of rewrite steps that
should be carried out.  For elucidation, each step may name the
particular rule which is applied to arrive at the transformed term
at that step.  Rules are named by their outermost constructor,
followed by a dot and the ordinal position of the rule in the list
of rules.  These rule-references are optional, but the fact that
the rule so named was actually used to rewrite the term at that step
could be checked too, of course.  The `qed` keyword ends the proof
block.

Naturally, you can also write a proof which does not hold, and
Madison should inform you of this fact.  2 + 3, for example,
does not equal 4, and it can pinpoint exactly where you went
wrong should you come to this conclusion:

| let
|   add(s(X),Y) -> add(X,s(Y))
|   add(z,Y)    -> Y
| in theorem
|   add(s(s(z)),s(s(s(z)))) ~> s(s(s(s(z))))
| proof
|   add(s(s(z)),s(s(s(z))))
|   -> add(s(z),s(s(s(s(z))))) [by add.1]
|   -> add(z,s(s(s(s(z)))))    [by add.1]
|   -> s(s(s(s(z))))           [by add.2]
| qed
? Error in proof [line 6]: step 2 does not follow from applying [add.1] to previous step

Now, while these *are* proofs, they don't tell us much about the
properties of the terms and rules involved, because they are not
*generalized*.  They say something about a few fixed values, like
2 and 4, but they do not say anything about any *infinite*
sets of values, like the natural numbers.  Now, that would be *really*
useful.  And, while I could say that what you've seen of Madison so far
is a proof checker, it is not a very impressive one.  So let's take
this further.

Quantification
--------------

To state a generalized proof, we will need to introduce variables,
and to have variables, we will need to be able to say what those
variables can range over; in short, we need *quantification*.  Since
we're particularly interested in making statements about infinite
sets of values (like the natural numbers), we specifically want
*universal quantification*:

    For all x, ...

But to have universal quantification, we first need a *universe*
over which to quantify.  When we say "for all /x/", we generally
don't mean "any and all things of any kind which we could
possibly name /x/".  Rather, we think of /x/ as having a type of
some kind:

    For all natural numbers x, ...

Then, if our proof holds, it holds for all natural numbers.
No matter what integer value greater than or equal to zero
we choose for /x/, the truism contained in the proof remains true.
This is the sort of thing we want in Madison.

Well, to start, there is one glaringly obvious type in any
term-rewriting language, namely, the term.  We could say

    For all terms t, ...

But it would not actually be very interesting, because terms
are so general and basic that there's not actually very much you
can say about them that you don't already know.  You sort of need
to know the basic properties of terms just to build a term-rewriting
language (like the one at Madison's core) in the first place.

The most useful property of terms as far as Madison is concerned is
that the subterm relationship is _well-founded_.  In other words,
in the term `c(X)`, `X` is "smaller than" `c(X)`, and since terms are
finite, any series of rewrites which always results in "smaller" terms
will eventually terminate.  For completeness, we should probably prove
that rigorously, but for expediency we will simply take it as a given
fact for our proofs.

Anyway, to get at something actually interesting, we must look further
than the terms themselves.

Types
-----

What's actually interesting is when you define a restricted
set of forms that terms can take, and you distinguish terms inside
this set of forms from the terms outside the set.  For example,

| let
|   boolean(true)  -> true
|   boolean(false) -> true
|   boolean(_)     -> false
| in
|   rewrite boolean(false)
= true

We call a set of forms like this a _type_.  As you can see, we
have basically written a predicate that defines our type.  If any
of the rewrite rules in the definition of this predicate rewrite
a given term to `true`, that term is of our type; if it rewrites
to `false`, it is not.

Once we have types, any constructor may be said to have a type.
By this we mean that no matter what subterms the constructor has,
the predicate of the type of which we speak will always reduce to
`true` when that term is inserted in it.

Note that using predicates like this allows our types to be
non-disjoint; the same term may reduce to true in two different
predicates.  My first sketches for Madison had disjoint types,
described by rules which reduced each term to an atom which named
the type of that term.  (So the above would have been written with
rules `true -> boolean` and `false -> boolean` instead.)  However,
while that method may be, on the surface, more elegant, I believe
this approach better reflects how types are actually used in
programming.  At the end of the day, every type is just a predicate,
and there is nothing stopping 2 from being both a natural number and
an integer.  And, for that matter, a rational number and a real
number.

In theory, every predicate is a type, too, but that's where things
get interesting.  Is 2 not also an even number, and a prime number?
And in an appropriate (albeit contrived) language, is it not a
description of a computation which may or may not always halt?

The Type Syntax
---------------

The above considerations motivate us to be careful when dealing
with types.  We should establish some ground rules so that we
know that our types are useful to universally quantify over.

Unfortunately, this introduces something of a chicken-and-egg
situation, as our ground rules will be using logical connectives,
while at the same time they will be applied to those logical
connectives to ensure that they are sound.  This is not, actually,
a big deal; I mention it here more because it is interesting.

So, the rules which define our type must conform to certain
rules, themselves.  While it would be possible to allow the
Madison programmer to use any old bunch of rewrite rules as a
type, and to check that these rules make for a "good" type when
such a usage is seen -- and while this would be somewhat
attractive from the standpoint of proving properties of term-
rewriting systems using term-rewriting systems -- it's not strictly
necessary to use a descriptive approach such as this, and there are
certain organizational benefits we can achieve by taking a more
prescriptive tack.

Viz., we introduce a special syntax for defining a type with a
set of rules which function collectively as a type predicate.
Again, it's not strictly necessary to do this, but it does
help organize our code and perhaps our thoughts, and perhaps make
an implementation easier to build.  It's nice to be able to say,
yes, what it means to be a `boolean` is defined right here and
nowhere else.

So, to define a type, we write our type rules in a `type..in`
block, like the following.

| type boolean is
|   boolean(true)  -> true
|   boolean(false) -> true
| in
|   rewrite boolean(false)
= true

As you can see, the wildcard reduction to false can be omitted for
brevity.  (In other words, "Nothing else is a boolean" is implied.)
And, the `boolean` constructor can be used for rewriting in a term
just like any other plain, non-`type`-blessed rewrite rule.

| type boolean is
|   boolean(true)  -> true
|   boolean(false) -> true
| in
|   rewrite boolean(tree(leaf(sabrina),leaf(joe)))
= false

Here are the rules that the type-defining rules must conform to.
If any of these rules are violated in the `type` block, the Madison
implementation must complain, and not proceed to try to prove anything
from them.

Once a type is defined, it cannot be defined further in a regular,
non-type-defining rewriting rule.

| type boolean is
|   boolean(true)  -> true
|   boolean(false) -> true
| in let
|   boolean(red) -> green
| in
|   rewrite boolean(red)
? Constructor "boolean" used in rule but already defined as a type

The constructor in the l.h.s. must be the same in all rules.

| type foo is
|   foo(bar) -> true
|   baz(bar) -> true
| in
|   rewrite cat
? In type "foo", constructor "bar" used on l.h.s. of rule

The constructor used in the rules must be arity 1 (i.e. have exactly
one subterm.)

| type foo is
|   foo(bar,X) -> true
| in
|   rewrite cat
? In type "foo", constructor has arity greater than one

It is considered an error if the predicate rules ever rewrite, inside
the `type` block, to anything besides the atoms `true` or `false`.

| type foo is
|   foo(bar) -> true
|   foo(tree(X)) -> bar
| in
|   rewrite cat
? In type "foo", rule reduces to "bar" instead of true or false

The r.h.s.'s of the rules of the type predicate must *always*
rewrite to `true` or `false`.  That means, if we can't prove that
the rules always rewrite to something, we can't use them as type
predicate rules.  In practice, there are a few properties that
we insist that they have.

They may involve type predicates that have previously been
established.

| type boolean is
|   boolean(true)  -> true
|   boolean(false) -> true
| in type boolbox is
|   boolbox(box(X)) -> boolean(X)
| in
|   rewrite boolbox(box(true))
= true

They may involve certain, pre-defined rewriting rules which can
be thought of as operators on values of boolean type (which, honestly,
is probably built-in to the language.)  For now there is only one
such pre-defined rewriting rule: `and(X,Y)`, where `X` and `Y` are
booleans, and which rewrites to a boolean, using the standard truth
table rules for boolean conjunction.

| type boolean is
|   boolean(true)  -> true
|   boolean(false) -> true
| in type boolpair is
|   boolpair(pair(X,Y)) -> and(boolean(X),boolean(Y))
| in
|   rewrite boolpair(pair(true,false))
= true

| type boolean is
|   boolean(true)  -> true
|   boolean(false) -> true
| in type boolpair is
|   boolpair(pair(X,Y)) -> and(boolean(X),boolean(Y))
| in
|   rewrite boolpair(pair(true,cheese))
= false

Lastly, the r.h.s. of a type predicate rule can refer to the self-same
type being defined, but *only* under certain conditions.  Namely,
the rewriting must "shrink" the term being rewritten.  This is what
lets us inductively define types.

| type nat is
|   nat(z)    -> true
|   nat(s(X)) -> nat(X)
| in
|   rewrite nat(s(s(z)))
= true

| type nat is
|   nat(z)    -> true
|   nat(s(X)) -> nat(s(X))
| in
|   rewrite nat(s(s(z)))
? Type not well-founded: recursive rewrite does not decrease in [foo.2]

| type nat is
|   nat(z)    -> true
|   nat(s(X)) -> nat(s(s(X)))
| in
|   rewrite nat(s(s(z)))
? Type not well-founded: recursive rewrite does not decrease in [foo.2]

| type bad
|   bad(leaf(X)) -> true
|   bad(tree(X,Y)) -> and(bad(X),bad(tree(Y,Y))
| in
|   rewrite whatever
? Type not well-founded: recursive rewrite does not decrease in [bad.2]

We can check this by looking at all the rewrite rules in the
definition of the type that are recursive, i.e. that contain on
on their r.h.s. the constructor being defined as a type predicate.
For every such occurrence on the r.h.s. of a recursive rewrite,
the contents of the constructor must be "smaller" than the contents
of the constructor on the l.h.s.  What it means to be smaller
should be fairly obvious: it just has fewer subterms.  If all the
rules conform to this pattern, rewriting will eventually terminate,
because it will run out of subterms to rewrite.

Application of Types in Proofs
------------------------------

Now, aside from these restrictions, type predicates are basically
rewrite rules, just like any other.  The main difference is that
we know they are well-defined enough to be used to scope the
universal quantification in a proof.

Simply having a definition for a `boolean` type allows us to construct
a simple proof with variables.  Universal quantification over the
universe of booleans isn't exactly impressive; we don't cover an infinite
range of values, like we would with integers, or lists.  But it's
a starting point on which we can build.  We will give some rewrite rules
for a constructor `not`, and prove that this constructor always reduces
to a boolean when given a boolean.

| type boolean is
|   boolean(true)  -> true
|   boolean(false) -> true
| in let
|   not(true)  -> false
|   not(false) -> true
|   not(_)     -> undefined
| in theorem
|   forall X where boolean(X)
|     boolean(not(X)) ~> true
| proof
|   case X = true
|     boolean(not(true))
|     -> boolean(true)   [by not.1]
|     -> true            [by boolean.1]
|   case X = false
|     boolean(not(false))
|     -> boolean(false)  [by not.2]
|     -> true            [by boolean.2]
| qed
= true

As you can see, proofs using universally quantified variables
need to make use of _cases_.  We know this proof is sound, because
it shows the rewrite steps for all the possible values of the
variable -- and we know they are all the possible values, from the
definition of the type.

In this instance, the cases are just the two possible values
of the boolean type, but if the type was defined inductively,
they would need to cover the base and inductive cases.  In both
matters, each case in a complete proof maps to exactly one of
the possible rewrite rules for the type predicate.  (and vice versa)

Let's prove the type of a slightly more complex rewrite rule,
one which has multiple subterms which can vary.  (This `and`
constructor has already been introduced, and we've claimed we
can use it in the definition of well-founded inductive types;
but this code proves that it is indeed well-founded, and it
doesn't rely on it already being defined.)

| let
|   and(true,true) -> true
|   and(_,_)       -> false
| in theorem
|   forall X where boolean(X)
|     forall Y where boolean(Y)
|       boolean(and(X,Y)) ~> true
| proof
|   case X = true
|     case Y = true
|       boolean(and(true,true))
|       -> boolean(true)        [by and.1]
|       -> true                 [by boolean.1]
|     case Y = false
|       boolean(and(true,false))
|       -> boolean(false)       [by and.2]
|       -> true                 [by boolean.2]
|   case X = false
|     case Y = true
|       boolean(and(false,true))
|       -> boolean(false)       [by and.2]
|       -> true                 [by boolean.2]
|     case Y = false
|       boolean(and(false,false))
|       -> boolean(false)       [by and.2]
|       -> true                 [by boolean.2]
| qed
= true

Unwieldy, you say!  And you are correct.  But making something
easy to use was never my goal.

Note that the definition of `and()` is a bit more open-ended than
`not()`.  `and.2` allows terms like `and(dog,cat)` to rewrite to `false`.
But our proof only shows that the result of reducing `and(A,B)` is
a boolean *when both A and B are booleans*.  So it, in fact,
tells us nothing about the type of `and(dog,cat)`, nor in fact anything
at all about the properties of `and(A,B)` when one or more of `A` and
`B` are not of boolean type.  So be it.

Anyway, since we were speaking of inductively defined types
previously, let's do that now.  With the help of `and()`, here is
a type for binary trees.

| type tree is
|   tree(leaf)        -> true
|   tree(branch(X,Y)) -> and(tree(X),tree(Y))
| in
|   rewrite tree(branch(leaf,leaf))
= true

We can define some rewrite rules on trees.  To start small,
let's define a simple predicate on trees.

| type tree is
|   tree(leaf)        -> true
|   tree(branch(X,Y)) -> and(tree(X),tree(Y))
| in let
|   empty(leaf)        -> true
|   empty(branch(_,_)) -> false
| in empty(branch(branch(leaf,leaf),leaf))
= false

| type tree is
|   tree(leaf)        -> true
|   tree(branch(X,Y)) -> and(tree(X),tree(Y))
| in let
|   empty(leaf)        -> true
|   empty(branch(_,_)) -> false
| in empty(leaf)
= true

Now let's prove that our predicate always rewrites to a boolean
(i.e. that it has boolean type) when its argument is a tree.

| type tree is
|   tree(leaf)        -> true
|   tree(branch(X,Y)) -> and(tree(X),tree(Y))
| in let
|   empty(leaf)        -> true
|   empty(branch(_,_)) -> false
| in theorem
|   forall X where tree(X)
|     boolean(empty(X)) ~> true
| proof
|   case X = leaf
|     boolean(empty(leaf))
|     -> boolean(true)  [by empty.1]
|     -> true           [by boolean.1]
|   case X = branch(S,T)
|     boolean(empty(branch(S,T)))
|     -> boolean(false) [by empty.2]
|     -> true           [by boolean.2]
| qed
= true

This isn't really a proof by induction yet, but it's getting closer.
This is still really us examining the cases to determine the type.
But, we have an extra guarantee here; in `case X = branch(S,T)`, we
know `tree(S) -> true`, and `tree(T) -> true`, because `tree(X) -> true`.
This is one more reason why `and(X,Y)` is built-in to Madison; it
needs to leverage what it means and make use of this information in a
proof.  We don't really use that extra information in this proof, but
we will later on.

Structural Induction
--------------------

Let's try something stronger, and get into something that could be
described as real structural induction.  This time, we won't just prove
something's type.  We'll prove something that actually walks and talks
like a real (albeit simple) theorem: the reflection of the reflection
of any binary tree is the same as the original tree.

| type tree is
|   tree(leaf)        -> true
|   tree(branch(X,Y)) -> and(tree(X),tree(Y))
| in let
|   reflect(leaf)        -> leaf
|   reflect(branch(A,B)) -> branch(reflect(B),reflect(A))
| in theorem
|   forall X where tree(X)
|     reflect(reflect(X)) ~> X
| proof
|   case X = leaf
|     reflect(reflect(leaf))
|     -> reflect(leaf)        [by reflect.1]
|     -> leaf                 [by reflect.1]
|   case X = branch(S, T)
|     reflect(reflect(branch(S, T)))
|     -> reflect(branch(reflect(T),reflect(S)))          [by reflect.2]
|     -> branch(reflect(reflect(S)),reflect(reflect(T))) [by reflect.2]
|     -> branch(S,reflect(reflect(T)))                   [by IH]
|     -> branch(S,T)                                     [by IH]
| qed
= true

Finally, this is a proof using induction!  In the [by IH] clauses,
IH stands for "inductive hypothesis", the hypothesis that we may
assume in making the proof; namely, that the property holds for
"smaller" instances of the type of X -- in this case, the "smaller"
trees S and T that are used to construct the tree `branch(S, T)`.

Relying on the IH is valid only after we have proved the base case.
After having proved `reflect(reflect(S)) -> S` for the base cases of
the type of S, we are free to assume that `reflect(reflect(S)) -> S`
in the induction cases.  And we do so, to rewrite the last two steps.

Like cases, the induction in a proof maps directly to the
induction in the definition of the type of the variable being
universally quantified upon.  If the induction in the type is well-
founded, so too will be the induction in the proof.  (Indeed, the
relationship between induction and cases is implicit in the
concepts of the "base case" and "inductive case (or step)".)

Stepping Back
-------------

So, we have given a simple term-rewriting-based language for proofs,
and shown that it can handle a proof of a property over an infinite
universe of things (trees.)  That was basically my goal in designing
this language.  Now let's step back and consider some of the
implications of this system.

We have, here, a typed programming language.  We can define types
that look an awful lot like algebraic data types.  But instead of
glibly declaring the type of any given term, like we would in most
functional languages, we actually have to *prove* that our terms
always rewrite to a value of that type.  That's more work, of
course, but it's also stronger: in proving that the term always
rewrites to a value of the type, we have, naturally, proved that
it *always* rewrites -- that its rewrite sequence is terminating.
There is no possibility that its rewrite sequence will enter an
infinite loop.  Often, we establish this with the help of previously
established basis that our inductively-defined types are well-founded,
which is itself proved on the basis that the subterm relationship is
well-founded.

Much like we can prove termination in course of proving a type,
we can prove a type in the course of proving a property -- such
as the type of `reflect(reflect(T))` above.  (This does not directly
lead to a proof of the type of `reflect`, but whatever.)

And, of course, we are only proving the type of term on the
assumption that its subterms have specific types.  These proofs
say nothing about the other cases.  This may provide flexibility
for extending rewrite systems -- or it might not, I'm not sure.
It might be nice to prove that all other types result in some
error term.  (One of the more annoying things about term-rewriting
languages is how an error can result in a half-rewritten program
instead of a recgonizable error code.  There seems to be a tradeoff
between extensibility and producing recognizable errors.)

Grammar so Far
--------------

I think I've described everything I want in the language above, so
the grammar should, modulo tweaks, look something like this:

    Madison      ::= Block.
    Block        ::= LetBlock | TypeBlock | ProofBlock | RewriteBlock.
    LetBlock     ::= "let" {Rule} "in" Block.
    TypeBlock    ::= "type" Symbol "is" {Rule} "in" Block.
    RewriteBlock ::= "rewrite" Term.
    Rule         ::= Term "->" Term.
    Term         ::= Atom | Constructor | Variable.
    Atom         ::= Symbol.
    Constructor  ::= Symbol "(" Term {"," Term} ")".
    ProofBlock   ::= "theorem" Statement "proof" Proof "qed".
    Statement    ::= Quantifier Statement | MultiStep.
    Quantifiers  ::= "forall" Variable "where" Term.
    MultiStep    ::= Term "~>" Term.
    Proof        ::= Case Proof {Case Proof} | Trace.
    Trace        ::= Term {"->" Term [RuleRef]}.
    RuleRef      ::= "[" "by" (Symbol "." Integer | "IH") "]".

Discussion
----------

I think that basically covers it.  This document is still a little
rough, but that's what major version zeroes are for, right?

I have essentially convinced myself that the above-described system
is sufficient for simple proof checking.  There are three significant
things I had to convince myself of to get to this point, which I'll
describe here.

One is that types have to be well-founded in order for them to serve
as scopes for universal quantification.  This is obvious in
retrospect, but getting them into the language in a way where it was
clear they could be checked for well-foundedness took a little
effort.  The demarcation of type-predicate rewrite rules was a big
step, and a little disappointing because it introduces the loaded
term `type` into Madison's vernacular, which I wanted to avoid.
But it made it much easier to think about, and to formulate the
rules for checking that a type is well-founded.  As I mentioned, it
could go away -- Madison could just as easily check that any
constructor used to scope a universal quantification is well-founded.
But that would probably muddy the presentation of the idea in this
document somewhat.  It would be something to keep in mind for a
subsequent version of Madison that further distances itself from the
notion of "types".

Also, it would probably be possible to extend the notion of well-
founded rewriting rules to mutually-recursive rewriting rules.
However, this would complicate the procedure for checking that a
type predicate is well-founded.

The second thing I had to accept to get to this conviction is that
`and(X,Y)` is built into the language.  It can't just be defined
in Madison code, because while this would be wonderful from a
standpoint of minimalism, Madison has to know what it means to let
you write non-trivial inductive proofs.  In a nutshell, it has to
know that `foo(X) -> and(bar(X),baz(X))` means that if `foo(X)` is
true, then `bar(X)` is also true, and `baz(X)` is true as well.

I considered making `or(X,Y)` a built-in as well, but after some
thought, wasn't convinced that it was that valuable in the kinds
of proofs I wanted to write.

Lastly, the third thing I had to come to terms with was, in general,
how we know a stated proof is complete.  As I've tried to describe
above, we know it's complete because each of the cases maps to a
possible rewrite rule, and induction maps to the inductive definition
of a type predicate, which we know is well-founded because of the
checks Madison does on it (ultimately based on the assumption that
the subterm property is well-founded.)  There Is Nothing Else.

This gets a little more complicated when you get into proofs by
induction.  The thing there is that we can assume the property
we want to prove, in one of the cases (the inductive case) of the
proof, so long as we have already proved all the other cases (the
base case.)  This is perfectly sound in proofs by hand, so it is
likewise perfectly sound in a formal proof checker like Madison;
the question is how Madison "knows" that it is sound, i.e. how it
can be programmed to reject proofs which are not structured this
way.  Well, if we limit it to what I've just described above --
check that the scope of the universal quantification is well-
founded, check that there are two cases, and check that we've already
proved one case, then allow the inductive hypothesis to be used as a
rewrite rule in the other case of the proof -- this is not difficult
to see how it could be mechanized.

However, this is also very limited.  Let's talk about limitations.

For real data structures, you might well have multiple base cases;
for example, a tree with two kinds of leaf nodes.  Does this start
breaking down?  Probably.  It probably breaks down with multiple
inductive cases, as well, although you might be able to get around
that with breaking the proof into multiple proofs, and having
subsequent proofs rely on properties proved in previous proofs.

Another limitation I discovered when trying to write a proof that
addition in Peano arithmetic is commutative.  It seemingly can't
be done in Madison as it currently stands, as Madison only knows
how to rewrite something into something else, and cannot express
the fact that two things (like `add(A,B)` and `add(B,A)`) rewrite
to the same thing.  Such a facility would be easy enough to add,
and may appear in a future version of Madison, possibly with a
syntax like:

    theorem
      forall A where nat(A)
        forall B where nat(B)
          add(A,B) ~=~ add(B,A)
    proof ...

You would then show that `add(A,B)` reduces to something, and
that `add(B,A)` reduces to something, and Madison would check
that the two somethings are in fact the same thing.  This is, in
fact, a fairly standard method in the world of term rewriting.

As a historical note, Madison is one of the pieces of fallout from
the overly-ambitious project I started a year and a half ago called
Rho.  Rho was a homoiconic rewriting language with several very
general capabilities, and it wasn't long before I decided it was
possible to write proofs in it, as well as the other things it was
designed for.  Of course, this stretched it to about the limit of
what I could keep track of in a single project, and it was soon
afterwards abandoned.  Other fallout from Rho made it into other
projects of mine, including Pail (having `let` bindings within
the names of other `let` bindings), Falderal (the test suite from
the Rho implementation), and Q-expressions (a variant of
S-expressions, with better quoting capabilities, still forthcoming.)

Happy proof-checking!  
Chris Pressey  
December 2, 2011  
Evanston, Illinois  
