Practical Matters
=================

This document is a collection of notes I've made over the years about the
practical matters of production programming languages — usually stemming
from being irked by some existing programming language's lack of adequate
(in my opinion) support for them.  As such, these thoughts may be overblown
and sophistry-laden.  But it is nice to have a place to put them.

Fundamental Abstractions
------------------------

The following facilities should be either built-in to the language, or part
of the standard (highly standardized) libraries:

* Tracing.  Ideally, the programmer should be able to easily browse all the
  relevant reduction steps, and the relevant data being manipulated therein,
  in the part of the program's execution that interests them.  In addition,
  this should be something that can be enabled without polluting the source
  code (overmuch).
  
  This could be done, and fairly well, with techniques from aspect-oriented
  programming.  The rules to describe what to trace (or to highlight in a
  full trace) could be specified in what amounts to a configuration file,
  and thus be an implementation issue rather than a language issue.
  
  Unfortunately, this ideal is hard to achieve, so the system should also
  support...

* Logging.  Logging is basically an ad-hoc way to explicitly achieve
  selective tracing: the programmer knows what points in the program, and
  what data, are of interest to them, and outputs that data to the log at
  those points.

  Whether this is "debug logging" during development, or to support post-
  mortem analysis of issues in production, it amounts to the same thing:
  debugging, just on different time scales.
  
  The use of a "log level" is mostly just a way to filter the trace built
  up in the log files.  This is not necessarily a bad idea, but it should
  probably not be linear; information should be logged based on the reason
  that it is being logged, probably in the form of some sort of "tag", and
  filterable on that (whether at the time the log is being recorded, or
  being read.)
  
  Logging should not count as a side-effect.

  The logging function itself should have some properties:

  - Should not have side-effects (for example from evaluating its arguments),
    so that if it is not executed (because we are not interested in that
    part of the execution trace) the behaviour of the program is not changed.

  - In fact, should ensure that its arguments have no side-effects, and
    ideally, be total, with no chance of hanging or crashing.

  - Should pretty-print the relevant values, include the type and other
    metadata of the values, and put clearly visible delimeters around the
    values so printed.

  - Should include the source filename and line number.

  - Should not be overridable (shadowed?  not sure what I meant here.)

* History.  This is more relevant in a language with mutable values, but
  as part of tracing, it is useful to know the history of mutations of a
  value.  With immutable values, it would be useful to be able to view
  all the reductions which fed into the computation of the value at a
  point.  Either way, however, this is expensive, so should be specified
  selectively.  Again, an external, aspect-like configuration language
  for specifying which values to watch makes this an implementation issue.

* Command-line option parsing.  This should not rely on the Unix or DOS
  idea of a command line, and it should be unified with parameter passing
  in the language itself; calling an executable built in the language with
  arguments `a b c` should be no different from calling a function from
  within the language with the arguments `a b c` (probably as string values.)

Reflection
----------

* First-class tracebacks.  When a program, for example, encounters an error
  parsing an external file such as a configuration file, it should be able to
  report the position in that file that caused the error as part of the
  traceback, for consistency.  Java has some limited facilities for this, and
  some Python libraries do this (Jinja2? werkzeug?) using frame hacks, but
  a less clumsy solution would be nice.

* Tracebacks are *not* a special case of logging, or an artefact of throwing
  exceptions.  Since the traceback is basically a formatted version of the
  current continuation, this suggests the two facilities should be unified,
  perhaps not totally, but to a high degree.

Abstractions, not Wrappers
--------------------------

The basic principle here is that the existing APIs of most libraries are
(let's be polite) less than ideal, especially when they were designed for
some other language (such as C), and instead of blindly wrapping them in a
new language, the designer should at least *try* to make something nicer.

The abstractions should also recognize that modern computer systems are
generally not resource-starved (or at least that truly high-level
programming languages should not treat them that way.)

This applies to very basic facilities as well as what are usually thought
of as external libraries.  Specifically,

* Date and time: We can do better than simply copycatting interfaces like
  `strftime`.  All time data should be stored consistently, in GMT, always
  with a time zone.

* String formatting: We can do better than simply copycatting interfaces
  like `printf`.  We can use visual formatting strings, where fixed-size
  slots appear as fixed-sized placeholders (of the same size) in the
  formatting string.  (See also the scathing prog21 criticism of the
  vertical tab character.)

* Line-oriented communication: We can look at line-oriented communication
  more generally, as a form of record-oriented communication where the
  "delimiter set" for each record is {LF, CR, CRLF}.

The programmer who really wants atavistic interfaces like those mentioned
above can always implement them as "compatibility modules" if they wish.

Seperation from the Implementation
----------------------------------

This is just a repeat of the above section in slightly different terms.

A language should avoid tying any language construct (e.g. imports,
include files) to the file system or the operating system.  Instead,
have mappings between e.g. module names and where they live in the file
system, and between our model of a running computer and a real OS.
These mapping could be  specified in configuration files which are
in the domain of the implementation and outside the domain of the
language, i.e. they never appear in programs.

Standard modules supplied with the language should expose *models* of
commonplace artefacts out in the world, for example operating systems.
The models are similar to the artefacts, in order that the burden of
implementing an interface from the model to any given artefact is not
too great.  However, the models are *not* the artefacts.  Programs
should be written to the model, not to the artefact.

People who construct bindings to the language should be encouraged
(only because they can't effectively be required) to create models
more abstract than the libraries that they are binding.

Insofar as possible, we can have a compiler optimize things so that they
match the underlying architecture.  The language should allows and even
encourage definitions in the most general sense; special cases are to be
detected and optimized when they occur, instead of instituting those
special cases into the language itself.

Another aspect of this point of philosophy is that it should be possible
to specify and change the performance characteristics of the program
(but ideally not its behaviour) from outside the program, using
configuration files.

This counts as a practical matter because maintaining code which is
cluttered with implementation-specific artefacts is burdensome.

Serialization
-------------

(This section needs to be rewritten)

- All primitive values must be serializable
- All primitive values must be round-trippable
- All primitive values must thus have an order to them (like Ruby 1.9's
  hashes) because in this world of representations, orderless things don't
  really exist
- When building user-defined values from primitive values it must be
  easy to retain these serialization properties in the composite value
- This is actually fairly agnostic of the particular serialization format
  (yaml, xml, binary, etc)
- S-expressions are trivially serializable, except for functions

Formatting
----------

Closely related to serialization.

Many languages support a "standard" operation to convert an arbitrary value to
a string.  Some even have two (e.g. Python's `str` and `repr`).

But in reality, there are any number of ways to convert a value to a string.
Why should the string representation of 16 necessarily be `"16"` — why not
`"0xf"` or `"XVI"`?  `"16"` is fine, but it should be explicitly noted to be
the default for the reason that it's the most convenient for the audience of
humans who use the decimal Arabic notation when dealing with numbers.

How can we support both a reasonable (and possibly configurable) default
formatting, as well as any number of other ways to format values which would
be more appropriate in different contexts?

Can we pass a "style" argument to the string-conversion function?

Should we establish a "design pattern" for writing formatting functions, and
provide support for implementing such patterns?

(Also, `format` is probably a better name for this function than `str`.)

Multiple Environments
---------------------

(This section needs to be rewritten)

- Lots of software runs in multiple environments - "development", "qa",
  "production"
- Inherently support that idea

Assertions
----------

(This section needs to be rewritten)

- Software engineering is more about defining invariants than writing code.
- An "assert" command which produces details errors in development, but only
  logs warnings in production environments
- Very lightweight so that programmers use it without thinking
  (Python's `self.assertEqual()` is *not* lightweight)
  (Erlang's `A = {foo,B}` IS lightweight)
- So a conditional, by itself, is an assertion. (?)

Interfaces
----------

(This section needs to be rewritten)

One way or another, it should be possible to discover (programmatically,
through reflection of some sort) the set of operations that a value supports —
its interface.  Each operation has a name and a signature of some sort.

Collections are interfaces.

Some parts of an interface might be "private".  This — information hiding —
is obviously a somewhat complex topic.  The obvious bit is that information
hiding is useful to prevent unintended changes to program state, but it also
hinders debugging and testing.

Usability
---------

Memorization is not a good thing to make programmers do.  This can be
addressed by either copying things from an existing language that the
programmer base can be expected to already have memorized, or by providing
a more orthogonal set of things which maps to the culture which programmers,
as people, already live in.  (For example, few people in the Western world
do not know that `&` means "and".)

Non-alphabetic symbols should, idealy, have the same meaning regardless of
the context they're used in — in other words, the language should avoid
using the same symbol for different purposes in different contexts.

(Lots of languages are lacking here.  In C, `*` is both multiplication and
dereferencing. In Python, `.` is both object attribute access and package
hierarchy — although packages are, at least, kind of like objects.  In Lua,
`=` is both assignment and key value association.)

Programming Languages vs. Operating Systems
-------------------------------------------

(this section needs to be cleaned up — not sure where to put it, and it
arguably doesn't belong here)

What you see before you in this distribution can be described as a
programming language, but many of the ideas took root while thinking about
operating systems.

What's the difference between a programming language and an operating system?

Well, maybe less than you think.

Programming languages do need to define the environment in which they can
express programs.  Sometimes this is a specific OS (like early C on Unix) --
or they claim to be "portable", but then they're really just defining an
abstraction against all the possible OS'es they think they'll run on.  Often
this abstract is clumsy, but some languages put a lot of thought into it,
like Smalltalk.

Operating systems, on the other hand, don't tell you what programming
language to use -- or do they?  A modern OS insists everything is, at some
point, in native machine language, and a running instance will almost always
be limited to a single machine language of a single architecture.  Somewhat
more alternative OS'es define a virtual machine language to abstract away
from the concrete machine language.  Usually this virtual machine language
looks like a machine language, but sometimes it's a tad more high-level,
like Lisp.  Any way you slice it, the OS does sanction a particular, albeit
usually low-level, programming language.

Where PL's and OS's seem to meet more-or-less neatly is in the idea of the
VM, so let's examine that.

Most modern virtual machines are designed to implement high-level languages
in a modern operating system environment.  The JVM was specifically designed
for running Java, and while .NET was ostensibly designed for multiple
languages, the bytecode is pretty closely tuned to C\#.

What these VMs were not designed to do, but what a VM "should really" be
designed to do (if it, at least, wants to live up to the name "virtual
machine") is to abstract the *hardware* and provide virtualizations
(abstractions) of the available devices.

An environment contains zero or more devices.  A device exposes zero
or more services.  Each service conforms to one or more interfaces.
Each service may additionally require one or more services be available
(by interface).

At one point I was calling this place where programming language and
operating system meet a "CE" (Computational Environment) because
"operating system" is far too generic-sounding and "programming language"
doesn't address the important environmental aspect here.  Whether I would
continue to use the term CE or not, I'm not sure — it could just add to the
confusion.

How do most programming languages deal with the abstraction of available
(or virtual) devices?  Terribly, I would say.  Take, as a simple example,
an addressable character screen device.  Someone writes a library, in C,
to access it (e.g. `ncurses`,) providing an API comprising C functions
and C structs.  Someone then writes a binding or a wrapper (e.g. using
`swig`) or otherwise foreign-function interfaces it to the language, usually
exposing the exact same C-level API naively adapted to the programming
language.  Then you, the programmer in this language, wrestle with working
with the device almost exactly as a C programmer would, initializing and
releasing it as a C programmer would, with limitations on how you may or
may not use it from multithreaded code like a C programmer would (which
might be brutally different from how the runtime for your programming
language implementation assumes that its world works.)  All this, with the
added hassle of having to make sure you have all these bindings for the
device for your chosen implementation of your language built and installed
correctly.
