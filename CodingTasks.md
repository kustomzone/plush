This is our "task card" list. When a user story from ActionPlan is taken on by a developer, it should be broken down into a set of small identifiable coding tasks. These are entered here, under a heading for the user story. See ActionPlan for more details on the process.

This list is meant to be very dynamic. Please edit it all the time, and keep it up to date. Add tasks as you think of more things to do, remove tasks that you decide aren't needed. Add stuff that you don't need to do right now, but don't want to forget. While a user story is being worked on you may want to keep the completed tasks here, but once the story is complete, you should feel free to just delete the lot.... That is what history is for!

# Tasks #

## A List ##

  * bugs in arithmetic expansion:
    * $(( sumSq += (sum += sample) **sample )) -- doesn't work right
    * $(( a /= 0 )) -- doesn't error correctly**

## B List ##

  * annotations are broken - can't hover over bars to show them any more
  * tab completion should always be responsive
    * need to kick off completion info immediately if stale
  * typeing a space in the input area is often sluggish or gets dropped

  * when starting via a server (--local or --remote), the output from the start-up processing ends up in the version line (!)

  * desktop icon (?)


## C List ##

  * Shell errors (usually errors from parsing, setting illegal shell state (like read-only vars), or errors from from special built-ins need to terminate a non-interactive shell, but return errors otherwise

  * re-run command "in place" - that is wipe the prior history item for it re-run it (possibly/probably in the original cd directory) and then just replace the output. Note sure if it moves down to the bottom or not (if so, animate?).

  * you should be able to select text in the window anywhere and copy followed immediately by paste should paste into the command input area (at end, or at last selection?) - not sure if this is possible


## Future / Still Unformed ##

  * Plush.Types should really be Plush.Parser.Command or some such

  * tests for ExitCode okay in for, if, while, etc...
    * needs to be refactored, and made consistent
    * does it handle errors, like command not found, correctly?

  * for loop doesn't handle the no-"in" case

  * get rid of all Handle use - it isn't good for a shell!
    * remove readUtf8File from Plush.Utilities

  * save/restore cd history? (when to prune? how to present?)

  * history pruning
    * stop saving (or remove?) output for commands with very long output (>100k?)
    * stop saving (or remove?) output for commands that go full screen

  * code for UTF8 decoding of read files is duplicated in too many places, refactor
  * code for scanning vial the PATH variable is in several places (search for "PATH"), refactor

  * input area while running
    * figure out why int and quit are ignored in the process
    * SUSP -> ^Z -> send SIGTSTP signal (18)

  * summary information should come from utilAnnotate, not annotate

  * setAnno needs writing...

  * need to capture control-c (or implement exit)

  * _mzero:_ "content expansion" for ENV, PS1, PS2, and PS4 vars
  * _mzero:_ "content expansion" for HERE docs

  * what is the bug that causes doctest final blocks with pipes to fail when running shelltest with plush?!??

  * review content of default profile and env scripts once plush has more abilities

  * PS1 prompt handling for web mode
  * PS2/PS4 prompt handling
    * in web mode
    * in terminal mode

  * refactorings
    * move json in/out back to stdin/stdout

  * rename recho to args (?)
  * rationalize exit codes

  * property handle redirections that on specials (or at least flag 'em)

  * built-ins that default to external command execution

  * normalize where to import <$> form: Data.Functor or Control.Applicative
    * use it instead of fmap (or even liftM?) on monadic actions

  * some more tests for character classes to reflect all the odd cases in §2.13, especially treatment of slash after open bracket, and hyphen at start - in filename.doctest

  * back history (commands before the first screenful) could load only when the user back scrolls, rather than in the background. It could be either directly on scrolling, or perhaps with a 'load more' button.

  * should clicking on the logo and/or name do anything?
  * redirect about link to http://thecomfyshell.org/ when that is built

  * add timestamp and or random number to query part of URL so browsers will reload even if only the hash changes?

## Done ##

  * ✓ normalize on using System.Exit's ExitCode type for process result

  * ✓ use start up dir as datadir if all else fails (will be nice for developers)
    * ✓ make dependent on developer build only
  * ✓ launch web page via open, or xdg-open command

  * ✓ properly handle variable assignments

  * ✓ embed the static file tree directly into the executable - see [file-embed](http://hackage.haskell.org/package/file-embed) as one possible method

  * ✓ export some bracket like function to work with exceptions

  * ✓ fix instance of Hashable for Location (Types.ps) so works with hashable-1.2

  * ✓ entering spaces to command input is broken (again!)
  * ✓ EOF to command input is broken (again!)

  * ✓ file names with white space don't display correctly in status area (see [issue #20](https://code.google.com/p/plush/issues/detail?id=#20))

  * ✓ history is too slow
    * need to recode to keep a separate history div
  * ✓ duplicates in history should be elided
    * on each re-entry, move down to the bottom

  * ✓ version/build number
    * from `plush --version` (or some such flag)
    * displayed somewhere in the front end page (static? loaded? via context command?)

  * ✓ visible link/button for bringing up help page
  * ✓ visible link to where to file bugs
  * ✓ visible about link to http://code.google.com/p/plush

  * ✓ need to get controlling terminal logic working so that things like ghci and other things that open /dev/tty work

  * ✓ alltests.sh should be more robust
    * ✓ only run for shells it can find
    * ✓ figure out which shell sh is

  * ✓ tab completion doesn't work relative to ~ (see [issue #22](https://code.google.com/p/plush/issues/detail?id=#22))

  * ✓ .plush/startup file at startup

  * ✓ code for reading a block of commands (from a file) is in both Main.hs and Evaluation.hs, refactor
  * history pruning at start up

  * ✓ update to new catch (?)