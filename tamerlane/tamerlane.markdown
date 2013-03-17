Tamerlane
=========

Chris Pressey  
Created Jan 29 2000

### Introduction to Tamerlane

Tamerlane is a "constraint flow" language. The point of its creation is
to attempt to break as many paradigmal stereotypes and idioms that I'm
aware of; at least, to make it tricky and confounding to pigeonhole
easily.

It has some concepts in it from imperative languages, some from
functional languages, some from dataflow and object oriented languages,
and some from graph rewriting and other constraint-based languages, and
they're all muddled together into a ridiculous *potpourri*.

Despite being such a mutt, there might actually be some algorithms that
would be dreadfully easy to write in Tamerlane.

### Overview of Tamerlane

A Tamerlane program consists of a mutably weighted directed graph.

Each node is considered to be an independent updatable store. The data
held in each store is represented by the weights of the arcs exiting the
node.

An arc of weight zero is functionally equivalent to the absence of an
arc.

### Description and Example

At this point we may introduce the syntax in a sample ASCII notation for
a simple, almost pathological Tamerlane program:

      Point-A: 1 Point-B,
      Point-B: 1 Point-C,
      Point-C: 1 Point-A.

The user of a Tamerlane program may submit *messages* to the program at
runtime. In this sense the user and the program are both objects which
share the symmetrical relationship **user-of/used-by**. The program
object's interface exposes a `query` method to the user, which is to be
considered runtime-polymorphic.

Using this message-passing mechanism, queries are submitted by the user
to a running Tamerlane program, much as queries would be submitted to a
running Prolog program.

Queries have their own syntax and semantics. Unlike Prolog, the user's
queries are interpreted as *rules*, perhaps accompanied by information
about where and when the rules are "introduced" into the graph.

As an example of a query that could be submitted to the above Tamerlane
program:

      1 Point-A -> 0 Point-A @ Point-A

This would introduce the rewriting rule

This rule is applied to the weights of the nodes in the graph starting
with the node specified after the `@` symbol. In this instance it would
start by trying to apply the rewrite to the node `Point-A`, but finding
`Point-A` to contain `1 Point-B`, nothing would change.

Each time a further query is submitted, each rule which has been
introduced into the graph, disappears from the node it was working on,
and is transmitted to the adjacent node with the lowest positive weight
value. If there is a tie for lowest weight, the rule is transmitted to
all of the adjacent nodes with the same lowest weight.

For efficacy we can consider the user able to submit a `nop` query. This
would not introduce any new rules into the graph, but it would cause all
active rules to propogate to new nodes on the graph.

So assume the user submits `nop`. The rule that was introduced by the
last query 'moves' from the node labelled `Point-A` to the node labelled
`Point-B` (since it's the only positive route out of `Point-A`.) It
tries to rewrite `Point-B`, but finding only `1 Point-C` in `Point-B`,
nothing happens.

Assume the user `nop`s again. The rule is propogated to `Point-C`.
Finally the pattern match succeeds, and `Point-C` is rewritten to a new
`Point-C`:

      Point-C: 0 Point-A.

After one more `nop`, the engine will generate a

      Rule stopped at Point-C (no adjacent nodes)

message back to the user. This uses the operation that the user object's
interface supplies to the running program, called `messageback`, which
the user must supply, but is, like `query`, considered
runtime-polymorphic.

Advanced Topics
---------------

### Rule Priority

Obviously, the user can enter more than one query in succession; s/he
doesn't need to explicity `nop` to wait for rules to resolve. Since the
graph can contain cycles, this would lead to a form of inherent,
synchronous concurrency: each time the `query` or `nop` methods are
invoked, more than one rule may be applied to the same node.

The order in which these competing rules is applied is based on the
rule's *priority*. Rules can be submitted with a specific priority in
the following manner:

      1 Point-A -> 0 Point-A @ Point-A ! 10

If there is a tie in priority when two rules are competing to rewrite
the same node, the outcome is guaranteed to be not simply undefined, but
rather, non-deterministic, or at least probabilistic.

The user can also specify a delay, measured in number of method calls
(`query`s or `nop`s) from the present query, at which the rule will be
introduced into the graph, like so:

      1 Point-A -> 0 Point-A @ Point-A in 10

And, for the sake of efficacy, the `nop` method on the program object
has overloaded syntaxes whose semantics are 'keep `nop`ing until some or
all rules have stopped.'

### Negative Weights

A negative weight from one node to another is interpreted in a rather
negative fashion with respect to the running program.

A negative weight is selected for propogation when it is closer to zero
(while still being non-zero) than any other weight (absolute value.)
When a negative weight is selected, however, the semantics of
propogation are different.

When a rule encounters an arc of the form:

      X: -1 Y.

The rule is not "just" copied to Y like "usual". Instead, it "rolls
back" the rule to Y in the fashion of a functional continuation.

All of the nodes that have been "touched" by this rule, since it was
last at node Y, are reset to their condition when the rule was last at
node Y. The rule itself is deleted from X and propogated to Y, and
rewriting continues from there.

If the rule has never visited Y, however, then such an exit arc is not
considered a valid candidate. It has an effective weight of 0
(non-existent.)

### Lambda Graphs

Lambda abstraction is a powerful form of referring to non-numeric
calculatory items such as functions and, in the case of Tamerlane,
graphs.

A user may submit a rule in the form

      1 X -> 1 Y ? Y: 1 Z, Z: 1 Y

Y is like a 'local variable' in this respect. When the rule is applied
to the node, and the pattern match succeeds, the nodes named Y and Z
need not actually exist, as they are simply created dynamically and
'attached' to the program graph.

Lambda graphs are subject to garbage collection, since they can only be
used by the rule that created them.

### Pigeonholes (Updatable)

Variables may be supplied which are explicitly updatable, and these are
termed pigeonholes. Pigeonholes acknowledge events. Their assignment is
associated with both nodes and arcs. They can occur when:

-   a rule enters a node
-   a rule leaves a node
-   a rule chooses an arc
-   a rule passes through an arc
-   a rule rewrites an arc

Updatable variables are always named after the node (or node.arc)
they're associated with, preceded by a `$` symbol. The data in the
variable is, of course, the arc weight (or in the case of nodes, the
node's internal key.)

If the pigeonhole is assigned the special value `%Cancel`, the rule is
cancelled from moving to the node/through the arc/rewriting the arc.

### Placeholders (Unification)

Variables may also be supplied as "placeholders". These are the same as
bindable (unifiable) variables in logical languages like Prolog. An
example of such might be:

      1 X 7 ^Y -> 7 X 1 ^Y

Which would replace "1 X 7 Foo" with "7 X 1 Foo", "1 X 7 Bar" with "7 X
1 Bar", etc.

### Horn Rules

Horn rules only succeed if all of their rules succeed. A Horn rule is
specified like:

      1 X -> 1 Y + 1 Z 0 G -> 1 G

(Remember that 0 G is equivalent to "the absence of an arc to G." If you
have a pattern like

      ^a G ^b G 0 G -> ^b Q

It will only match when there are exactly two different exit arcs to the
same node G, which is not disallowed (nor is a node with an exit arc
which points to itself.))

Note also that arcs are unordered amongst themselves. The rule

      1 A 1 B 1 C -> 1 E 3 D

is the same as

      1 B 1 A 1 C -> 3 D 1 E
