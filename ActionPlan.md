This is our high-level action plan for plush. It lists the user visible features that we plan to complete for each milestone. The description here (even if brief) should be enough for us to know what we are expecting plush to do when it is complete. It is acceptable to add some details, if the feature has several parts. When a user story is broken down into a list of coding tasks, they should be placed on the CodingTasks page.

If you are taking responsibility for a user story, then put your name next to it in _italics_. This means that you're taking lead on the design of the feature, and will break it down into parts (on CodingTasks, please). At that point, others can help you code it if agreeable. Leave the user story here, marking it done when complete. Feel free to split a user story into parts (if the features can be completed independently), and take only a part. We'll clean out this list later, as we declare milestones done.

# Milestone 2 #

The aim is something other developers could use.

  * cards
    * ✓ status panes
    * git status view of current directory if .git present
    * hg status view of current directory if .hg present

  * ls returns file objects json
    * files shown as icons
    * hovering gives info
    * hover & space gives preview
    * preview: text, html, images

  * missing POSIX features
    * ✓ backtick & subcommands substitution
    * ✓ parameter substitution expressions
    * ✓ here docs
    * ✓ special parameters $@, $$, $!, and $0
    * ✓ if
    * ✓ while / until
    * ✓ case
    * ✓  subshells
    * ✓ break / continue / return / exit
    * ✓ shell errors
    * ✓ read
    * newgrp / umask

  * multi-line input
  * tab completion improvements
    * vi/emacs mode
  * copy/paste
  * rerun command / drop command
  * command meta info: date, working dir

  * ✓ server control: start / stop / status / launch
    * ✓ --remote story
    * ✓ --local story

  * desktop icons

  * website



# Milestone 1 #

The aim is for the developers to be able to use it themselves for day to day shell usage. A secondary aim is to have a kick-ass demo.

  * ✓ pipes
  * ✓ ~ expansion
  * ✓ three-phase collapsing job panes
  * ✓ input to running commands
  * ✓ auto-less: long output is placed in 3-state div
    * (collapsed to 1 line, 10 lines w/scroller, full)
  * ✓ pass input to non-full screen, interactive commands

  * ✓ midground tasks, with live output
  * ✓ auto-midground tasks (if not shell affecting)
  * ✓ indication of input line or command input
  * ✓ throttle client side probing

  * ✓ deal with pty issues
    * ✓ no way to quit/kill a task probably due to process groups

  * ✓ tab completion of file names

  * ✓ keyboard job view control
    * move focus up/down jobs (+shift for running jobs?)
    * move focus back/forth from command input
    * commands for resizing job output (<sup>0, </sup>1, <sup>2, </sup>3 ?)
    * commands for EOF/Quit/Kill (<sup>D, </sup>C, ^\ ?)
    * command to re-enter command from job
    * apply commands to last job (recent?) when on command input

  * ✓ scrolling control
    * page up / down selected job's output
    * home / end selected job's output
    * auto-pager: don't scroll fast output, let user page it

  * ✓ tab completion of commands

  * ✓ command history review and re-entry

  * ✓ job history persistence

  * ✓ key shortcut help screen
    * ✓ key mode system

  * ✓ open web page option
    * ✓ use open or xdg-open

  * ✓ export support (and unset and readonly)
  * ✓ environment vars on command line:  foo=bar cmd args

  * ✓ for loop

  * ✓  ls status pane: view of current directory

  * ✓ cd stack: current dir indicator has a pop-up list of prior and likely cd targets
    * - selecting one cds there (entering command into history)

  * ✓ function support

  * ✓ alias support

  * ✓ tab completion UI
    * arrows, and modal key capture

  * ✓ packaging up for easy installation
    * ✓  include static files within executable itself (?)

  * full-screen mode
    * ✓ vt100 support
    * reconcile auto-resizing & job-size buttons
    * styling - background, placement, etc...
    * quit/kill buttons
    * freeze & collapse on exit (heuristics)
      * vi on exit should collapse entirely
      * ls -G on exit should leave output

  * input focus
    * no indication when input focus is on vt100 screen
    * input focus for running job should follow job focus

  * start up rc files for plush - .plush\_profile , .profile , etc...
    * - perhaps with some clearer logic that bash!

  * easy/canonical way to launch plush on a remote system

## Maybe ##

  * remove job from screen and history (and undo the remove)

## Whizzy ##

  * tab completion of options
  * peel-off

  * _marius:_ cd by double clicking on file icon (from ls json out)


# For later milestones #

  * support the exit command
  * control-C plush itself

  * less plush enabled: pager in the page
  * find returns file objects (!)

  * backgrounding
  * kill background tasks
  * suspend background tasks
  * pass input to background tasks

  * >>= operator on command line:  a >>= b  is   a | xargs b
    * json based support?
    * not sure how to handle n-at a time
  * stash buffer for commands:
    * idiomatic bash is the # command: put a # in the front and hit return
    * thus saving in history, but not executing
  * test support