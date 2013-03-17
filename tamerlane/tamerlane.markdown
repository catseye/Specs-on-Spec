<html><head>
    <meta http-equiv="Content-Type" content="text/html;CHARSET=iso-8859-1">
    <meta name="Description" content="Cat's Eye Technologies: The Tamerlane Programming Language">
    <title>Cat's Eye Technologies: The Tamerlane Programming Language</title>
</head>
<body  bgcolor="#ffffff" text="#000000" link="#1c3f7f"
      vlink="#204020" background="/img/sinewhite.gif" >
<center>
<h1>Tamerlane</h1>
<p>Chris Pressey
<br>Created Jan 29 2000
</center>

<h3>Introduction to Tamerlane</h3>

<p>Tamerlane is a "constraint flow" language.  The point of its creation
is to attempt to break as many paradigmal stereotypes and idioms that
I'm aware of; at least, to make it tricky and confounding
to pigeonhole easily.

<p>It has some concepts in it from imperative languages,
some from functional languages, some from dataflow and object oriented
languages, and some from graph rewriting
and other constraint-based languages, and they're all muddled together
into a ridiculous <i>potpourri</i>.

<p>Despite being such a mutt, there might actually be some algorithms that
would be dreadfully easy to write in Tamerlane.

<h3>Overview of Tamerlane</h3>

<p>A Tamerlane program consists of a mutably weighted directed graph.

<p>Each node is considered to be an independent updatable store.
The data held in each store is represented by the weights of the
arcs exiting the node.

<p>An arc of weight zero is functionally equivalent to the absence
of an arc.

<h3>Description and Example</h3>

<p>At this point we may introduce the syntax in a sample ASCII
notation for a simple, almost pathological Tamerlane program:

<pre>
  Point-A: 1 Point-B,
  Point-B: 1 Point-C,
  Point-C: 1 Point-A.
</pre>

<p>The user of a Tamerlane program may submit <i>messages</i>
to the program at runtime.  In this sense the user and the program are
both objects which share the symmetrical relationship
<b>user-of/used-by</b>.  The program object's interface exposes a
<tt>query</tt> method to the user, which is to be considered
runtime-polymorphic.

<p>Using this message-passing mechanism, queries are submitted by the user to a
running Tamerlane program, much as queries would be submitted
to a running Prolog program.

<p>Queries have their own syntax and semantics.
Unlike Prolog, the user's queries are interpreted as <i>rules</i>,
perhaps accompanied by information about
where and when the rules are "introduced" into the graph.

<p>As an example of a query that could be submitted to the above
Tamerlane program:

<pre>
  1 Point-A -> 0 Point-A @ Point-A
</pre>

This would introduce the rewriting rule

<ul>"Replace occurances of <tt>1 Point-A</tt> with <tt>0 Point-A</tt>"
</ul>

<p>This rule is applied to the weights of the nodes in the graph
starting with the node specified after the <tt>@</tt> symbol.  In this
instance it would start by trying to apply the rewrite to
the node <tt>Point-A</tt>, but finding <tt>Point-A</tt> to contain <tt>1 Point-B</tt>,
nothing would change.

<p>Each time a further query is submitted, each rule which has
been introduced into the graph, disappears from the node it
was working on, and is transmitted to the adjacent node with
the lowest positive weight value.  If there is a tie for
lowest weight, the rule is transmitted to all of the
adjacent nodes with the same lowest weight.

<p>For efficacy we can consider the user able to submit a <tt>nop</tt>
query.  This would not introduce any new rules into the graph,
but it would cause all active rules to propogate to new nodes
on the graph.

<p>So assume the user submits <tt>nop</tt>.  The rule that was
introduced by the last query 'moves' from the
node labelled <tt>Point-A</tt> to the node labelled <tt>Point-B</tt> (since it's
the only positive route out of <tt>Point-A</tt>.)  It tries to rewrite
<tt>Point-B</tt>, but finding only <tt>1 Point-C</tt> in <tt>Point-B</tt>, nothing happens.

<p>Assume the user <tt>nop</tt>s again.  The rule is propogated to <tt>Point-C</tt>.
Finally the pattern match succeeds, and <tt>Point-C</tt> is rewritten to
a new <tt>Point-C</tt>:

<pre>
  Point-C: 0 Point-A.
</pre>

<p>After one more <tt>nop</tt>, the engine will generate a

<pre>
  Rule stopped at Point-C (no adjacent nodes)
</pre>

<p>message back to the user.  This uses the operation that the user object's
interface supplies to the running program, called <tt>messageback</tt>,
which the user must supply, but is, like <tt>query</tt>, considered
runtime-polymorphic.

<h2>Advanced Topics</h2>

<h3>Rule Priority</h3>

<p>Obviously, the user can enter more than one query in
succession; s/he doesn't need to explicity <tt>nop</tt> to
wait for rules to resolve.  Since the graph can contain cycles, this would lead
to a form of inherent, synchronous concurrency: each time the <tt>query</tt> or <tt>nop</tt>
methods are invoked, more than one rule may be applied to the same
node.

<p>The order in which these competing rules is applied is based
on the rule's <i>priority</i>.  Rules can be submitted with a specific
priority in the following manner:

<pre>
  1 Point-A -> 0 Point-A @ Point-A ! 10
</pre>

<p>If there is a tie in priority when two rules are competing to
rewrite the same node, the outcome is guaranteed to be
not simply undefined, but rather, non-deterministic, or at least
probabilistic.

<p>The user can also specify a delay, measured in number of method calls
(<tt>query</tt>s or <tt>nop</tt>s)
from the present query, at which the rule will be introduced into
the graph, like so:

<pre>
  1 Point-A -> 0 Point-A @ Point-A in 10
</pre>

<p>And, for the sake of efficacy, the <tt>nop</tt> method on the
program object has overloaded syntaxes whose semantics are
'keep <tt>nop</tt>ing until some or all rules have stopped.'

<h3>Negative Weights</h3>

<p>A negative weight from one node to another is interpreted in a
rather negative fashion with respect to the running program.

<p>A negative weight is selected for propogation when it is closer
to zero (while still being non-zero) than any other weight (absolute
value.)  When a negative weight is selected, however, the
semantics of propogation are different.

<p>When a rule encounters an arc of the form:

<pre>
  X: -1 Y.
</pre>

<p>The rule is not "just" copied to Y like "usual".  Instead, it
"rolls back" the rule to Y in the fashion of a functional
continuation.

<p>All of the nodes that have been "touched" by this rule, since it
was last at node Y, are reset to their condition when the rule
was last at node Y.  The rule itself is deleted from X and
propogated to Y, and rewriting continues from there.

<p>If the rule has never visited Y, however, then such an exit arc is
not considered a valid candidate.  It has an effective weight of
0 (non-existent.)

<h3>Lambda Graphs</h3>

<p>Lambda abstraction is a powerful form of referring to non-numeric
calculatory items such as functions and, in the case of Tamerlane, graphs.

<p>A user may submit a rule in the form

<pre>
  1 X -> 1 Y ? Y: 1 Z, Z: 1 Y
</pre>

<p>Y is like a 'local variable' in this respect.  When the rule is
applied to the node, and the pattern match succeeds, the nodes
named Y and Z need not actually exist, as they are simply
created dynamically and 'attached' to the program graph.

<p>Lambda graphs are subject to garbage collection, since they can
only be used by the rule that created them.

<h3>Pigeonholes (Updatable)</h3>

<p>Variables may be supplied which are explicitly updatable,
and these are termed pigeonholes.  Pigeonholes acknowledge events.
Their assignment is associated with both nodes and arcs.  They
can occur when:

<ul>
<li>a rule enters a node
<li>a rule leaves a node
<li>a rule chooses an arc
<li>a rule passes through an arc
<li>a rule rewrites an arc
</ul>

<p>Updatable variables are always 
named after the node (or node.arc) they're associated with, preceded by
a <tt>$</tt> symbol.  The data in the variable is, of course, the arc weight
(or in the case of nodes, the node's internal key.)

<p>If the pigeonhole is assigned the special value <tt>%Cancel</tt>,
the rule is cancelled from moving to the node/through the arc/rewriting the
arc.

<h3>Placeholders (Unification)</h3>

<p>Variables may also be supplied as "placeholders".  These are the
same as bindable (unifiable) variables in logical languages like
Prolog.  An example of such might be:

<pre>
  1 X 7 ^Y -> 7 X 1 ^Y
</pre>

<p>Which would replace "1 X 7 Foo" with "7 X 1 Foo", "1 X 7 Bar" with
"7 X 1 Bar", etc.

<h3>Horn Rules</h3>

<p>Horn rules only succeed if all of their rules succeed.  A Horn rule
is specified like:

<pre>
  1 X -> 1 Y + 1 Z 0 G -> 1 G
</pre>

<p>(Remember that 0 G is equivalent to "the absence of an arc to G." If you
 have a pattern like

<pre>
  ^a G ^b G 0 G -> ^b Q
</pre>

<p>It will only match when there are exactly two different exit arcs to
the same node G, which is not disallowed (nor is a node with an exit
arc which points to itself.))

<p>Note also that arcs are unordered amongst themselves.  The rule

<pre>
  1 A 1 B 1 C -> 1 E 3 D
</pre>

<p>is the same as

<pre>
  1 B 1 A 1 C -> 3 D 1 E
</pre>

<hr>
<center><font size=-1>Last Updated Jan 27&nbsp;<img src="/img/copyright.gif"
   align=absmiddle width=12 height=12 alt="(c)" border=0>2001 <a href="/">Cat's Eye Technologies</a>.</font></center>
</body></html>
