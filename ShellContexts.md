This is the design page for an upcoming feature in Plush.

# User View #

A _context_ is somewhat half-way between `cd`ing to a new directory, and spawning a whole new shell. It gives you the ability to maintain several work flows, but stay within a single shell and window.

You start off in the 'home' context. At anypoint, when `cd`'d into some other working directory, you can create a new context. The new context is anchored at this starting working directory, and maintains its own notion of current and previous working directories. `cd` works as expected except that a bare `cd` returns you to the anchor directory of the current context.

Switching to an existing context switches to that context's current working directory. It also restores that context's idea of anchor and previous working directories.

## User Interface ##

Within the plush UI, contexts are shown as a set of tab like controls above the screen area. The reflect the current available contexts, and which one is active. Each context has a color, and that color is used to paint the backgrounds command output, and the input area.

The tabs operate somewhat like preset buttons on a radio: There are just six of them at the top, always present, though some may be un-set. Single click switches context. Press-and-hold creates a new context with the current working directory as the anchor, overwriting any context that was in that tab already. Hover shows details. There is a circled-x button in the tab (when hovering) for deleting a context (leaving it blank). The first context is the "home" context and can't be overwritten or deleted.

## Command Execution ##

Each executed command remembers the context and working directory that it was executed in. The background of that command's output is colored with the context's color. When showing the previous commands, and when searching history, the list can be either inclusive of all previous commands, or restricted to just commands from the current context.

When re-executing a command from history, the user now has options to re-execute it in the context and working directory that it was originally executed in, rather than in the current context and working directory. This is useful even if you are in the same context: Often you want to re-execute a build command from the same place in the file system, even if you've `cd`'d someplace deeper in the tree.

# Shell Changes #



# Design Questions #

If a context is destoryed, and then a new context created using the same "slot" (and hence color), are the history of commands in these two contexts joined, or are they kept distinct. In otherwords, are contexts in history represented by the "slot" ("#3, the orange context"), or do they have some unique id, which is mapped to a slot and color ("#1923-1a2f, which was in slot #3 and orange")?