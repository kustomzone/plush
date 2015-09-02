$summary notes on Expansion

# Which Expansions are Performed When #

The spec has many contesxts in which expansion occurs, and they are often subtly different.

## Variables ##

The varibles `ENV`, `PS1`, `PS2`, and `PS4` are subject to Paremter Expansion before each use. bash and dash seem to do Command and Arithmetic Expansion as well

## Redirection ##

The word following a redirection operator is subject to only Parameter, Command,
Arithmetic Expansions, and Quote Removal. Pathname Expansion is expressly
ommitted for a non-interactive shell. It is okay for an interactive shell, but
only if one word results. (!)

Oddly, only Quote Removal is required for `<<` and `<<-` operators.

In "Here-Documents", if none of the characters in the sentinel word were quoted
(!), _then_, lines are subject to Parameter, Command, and Arithmetic Expansions.
In this case, the backslash escapes dollar, back-tick, backslash, and newline.
Notably not double quote - except when within another construct.

## Commands ##

The word in a `case` construct, as well as all the patterns are subject to Tilde, Parameter, Command, and Arithmetic Expansion.
