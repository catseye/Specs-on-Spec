Irishsea: Model
===============

The Irishsea model (communications/control model, or execution environment)
comprises a set of concurrently-executing processes.

Each process may receive messages, and may send messages to other processes.

Devices look like any other processes in this model.  Input from them is
sent to some other process as a message.  Sending them messages causes them
to produce output, or to engage in other activities, possibly observable.

The Irishsea environment is one such "input device".  Instructions in the
Irishsea language are entered into it; such instructions typically command
it to send a specified message to a specified process.

Irishsea processes may be implemented in any programming language; it is not
necessary for it to be the Irishsea language.  However, such processes must
conform to the semantics of (i.e. expected behaviour of) Irishsea processes,
which will now be described.

...
