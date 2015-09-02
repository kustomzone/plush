These are miscellaneous development notes.

# Parsing Tokens #

§2.3 as written:

  * there is a token-in-progress ("tip")
  * we are looking at next character ("c")
  * "delim" means, if tip isn't empty, emit it as a token, and set tip to empty
  * "cycle c" means, restart with character c on the next pass

first match wins:

  1. if EOF, delim
  1. if tip is an operator, and c isn't quoted and tip+c is an operator,
> > append c to tip
  1. if tip is an operator (and ##didn't match), delim & cycle c
  1. if c is \, ', or " -- slurp the quoted text, and append to tip
  1. if c is $ or ` -- slurp the parsed text, and append to tip
  1. if c isn't quoted, and can start an operator, delim, set tip to c
  1. if c is an unquoted newline, delim & cycle c
  1. if c is an unquoted blank, delim & discared c
  1. if tip isn't empty and isn't an operator, append c to tip
  1. if c is #, discared it and all upto (but not including) the next newline
  1. set tip to c

§2.10.1 says:

  1. a newline returns as a NEWLINE token
  1. operators are TOK\_XXX
  1. if string is only digits, and immediately folled by < or >, then IO\_NUMBER
  1. else TOKEN, which can in turn be WORD, NAME, ASSIGNMENT, or reserved word


Seems to me that §2.10 happens first, then §2.3 once one has a TOKEN, though operator parsing seems mixed in stragely --- the issue is newline, which I'm going to take as always representing a single token of itself (except when removed via backslash...)


---


what we want is a set of rules written the other way 'round: starting from an empty tip, what do we do until we get a delim?

<pre>
if tip is empty then<br>
if EOF - tok_eof<br>
if newline - tok_newline<br>
if starts an operator - start an op<br>
which then is as many characters as will keep making an op<br>
if blank - drop it and cycle<br>
if # - drop upto next newline, consume that, return tok_newline<br>
else set tip to c and append based on:<br>
if EOF - stop<br>
if \, ', or " - slurp quoted text onto tip<br>
if $ or ` - slurp parsed text onto tip<br>
if starts an operator - stop<br>
if newline - stop w/o using it<br>
if blank - stop, discarding it<br>
otherwise - appned to tip<br>
</pre>

for WORD:
  * c can't be EOF, newline, operator starts
  * continues so long as not EOF, operator starts, newline, blank
    * note that \ ' " $ ` are all processed specially

# Execution #

six cases:
  * special built-in
  * function
  * affecting built-in
  * regular built-in
  * executable
  * script


# Completion #
Things that can be completed:
  * command name (first word)
  * args, generically as file paths (2-n words)
  * args, as options (if word starts with '-')
  * words in assignment right hand sides
  * words in redirection right hand sides (for some redir types)
  * variable names in parameter words

  * word as expanded or word as entered?
  * command specific completions?


  * bash completes:
    * $xxx as a variable
    * ~xxx as a user name
    * @xxx as a host name
    * command name (if first word?)
    * file name

  * bash basically runs EITHER command driven completion - OR default
  * never both!

  * plenty of corner cases!!!


# Setting up Jenkins build #

compiler is an axis with values like:
  * ghc-6.12.3
  * ghc-7.0.3
  * ghc-7.2.1
  * ghc-7.4.1

the build script is something like:
```
    cabal install -w $compiler --only-dependencies --enable-tests
    cabal clean
    cabal configure -w $compiler --enable-tests
    cabal build
    cabal test --test-option=--plain
    cabal sdist
```
OR
```
    cabal install -w $compiler --only-dependencies
    cabal clean
    cabal configure -w $compiler
    cabal build
    cabal sdist
```



# §2.12 Shell Execution Environment #

Shell (and special built ins) have:
  * open files (numbered handles)
  * working directory
  * file creation mask
  * traps
  * variables (name -> (extern?, readonly?, value)
  * functions
  * set options
  * async processes
  * aliases

Utilities have:
  * open files (numbered handles)
  * working directory
  * file creation mask
  * traps (if a shell script?!?!?!)
  * env. variables (name -> value)
  * + system interfaces