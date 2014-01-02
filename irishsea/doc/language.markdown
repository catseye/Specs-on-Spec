Irishsea: Language
==================

The Irishsea language provides a syntax and semantics for instructing an
Irishsea process to send messages to other processes, and how to react to
messages it receives.

The Irishsea language is extremely terse.  It is designed to be economical
to enter on a US QWERTY computer keyboard layout, rather than to be readable.
(However, many constructs may have an alternate "long form" for readability,
when speed of entry is not an issue.)

(TODO the language should probably be somewhat flexible on the point of
keyboard layout, for non-US keyboards, perhaps by letting symbols be
redefined.)

It assumes the following characters can be entered with a single keystroke,
with no modifier keys: lower-case Latin letters from `a` to `z`, decimal
digits from `0` to `9`, and the following symbols: `` ` `` (backquote),
`-` (hyphen), `=` (equals sign), `[` and `]` (square brackets), `\\`
(backslash), `;` (semicolon), `'` (apostrophe), `,` (comma), `.` (period),
`/` (forward slash), and ` ` (blank space).

(A keyboard with a numeric keypad may also provide `+` and `*`, but the
presence of a numeric keypad should not be assumed.)

It reserves these characters for use in language constructs which must be
issued frequently and quickly.  Other characters are relegated to those
constructs which are less common or which are rarely issued "on-line".

...
