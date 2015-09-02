# Terminal Handling #

Each midground job needs:
  * a) it's own pair of pttys (one for stdin/out, one for stderr)
  * b) to be it's own process group, with first process as process group number
  * c) to have the stdin/out ptty as the controlling terminal
  * d) to have it's own process group as the control terminal process group
  * e) (on linux) have the first process be the session leader

c) is the hard one - how to make the pty the controlling terminal

### Functions ###

`setsid()`: "is used to create a new session containing a single (new) process group, with the current process as both the session leader and the process group leader of that single process group."

`setpgid()`: "is used to set the process group ID of a process, thereby either joining the process to an existing process group"

`tcsetpgrp()`: set the terminal's foreground process group

### Status ###

Complete in commit r519f6d7 (though should verify b & d)


# Inherited FDs #

`lsof` reveals a horrible FD environment. Looking at `cat | rev` we find:
  * spawned commands (cat, rev, etc) have
    * have /dev/null open on 5 ~ 9
    * have the TCP listener, and the TCP socket open on high FDs
    * have an extra copy of the PIPE open in cat
    * have jsonout/in hooked up wrong
  * forking plushes (the useless ones directly above cat and rev)
    * have all that open plus additional pipes
  * all have something called 'systm' open on FD15

### Details ###

**After kicking off shell thread:**

| 0,1,2 | slaves for pseudottys |
|:------|:----------------------|
| 3,5~9 | /dev/null place holders |
| 4     | jsonout pipe (slave, write end) |
| 10~14 | ghcrts                |
| 15,17 | masters for pseudyttys |
| 19    | jsonout pipe (master, read end) |
| 20~22 | relocated original stdin/stdout/stderr |
| 23    | permenant /dev/null   |

**After starting web server,** as above plus:

| 16 | ??? "a SYSTEM domain socket AF\_SYSTEM" |
|:---|:----------------------------------------|
| 18 | main listening port                     |
| 21 | now closed (was relocated original stdout(?)) |

**While running midground,** as above plus:

| 21    | misc. http client connection |
|:------|:-----------------------------|
| 29,31 | masters for pseudotty to midground |
| 30,32 | history logging for midground |
| 33    | jsonout pipe (master, reading end) |

**In midground fork:** as above

### To Do ###

  * - should have marked haskeline's FD as close-on-exec (`FD_CLOEXEC`)
    * ☹ Can't be helped! See [Haskeline bug #123](http://trac.haskell.org/haskeline/ticket/123)
  * ✓ pipes aren't being closed correctly
  * ✓ should be marked `FD_CLOEXEC`:
    * ✓ 15,17 -- masters for pseudyttys
    * ✓ 19    -- jsonout pipe (master, read end)
    * ✓ 20~22 -- relocated original stdin/stdout/stderr
  * ✓ maybe be marked `FD_CLOEXEC`:
    * ✓ 23    -- permenant /dev/null
  * ✓ web server should have these marked `FD_CLOEXEC`:
    * ✓ 16 -- ??? "a SYSTEM domain socket AF\_SYSTEM"
    * ✓ 18 -- main listening port
    * ✓ 21,24~28 -- various client HTTP ports

Note: `FD_CLOEXEC` doesn't work for `fork` (`forkProcess` in Haskell),
only the `exec` family

Work above completed in rbe84321 r7b39597 r287cb47 rc28782e

The FD 16 mystery is **solved**! It is a connection to a Mach OS kernel network
extension. Warp, by default, uses `bindPort` from Data.Conduit.Network. That
does to do a lot of work to find a suitable address, which causes `getAddrInfo`
to open up various network devices. By specifying the host as "127.0.0.1", this
library pathway does less work, and binds to just that address (which is
something we've been meaning to do anyway) - and _doesn't open this FD!_
Fixed in r1e6aab1


# Setting up stdin/stdout/stderr #

The redirection code and the pty code both do their manipulations of fds in
the parent before the fork (or exec), then undo them once the child has
been spawned. Most shells, I think, would do this work in the child.

The difficuty here for us is that we don't know until late if the
action is going to be "local" or fork'd.

This makes the case that "local" actions should use some other mechanism
to operate rather than the real environment - in particular with respect
to FDs. That is - if `SpecialUtility` didn't have access to FDs, then the
shell's FD environment wouldn't need to be modified. Instead, things like
outStr would need to go through code that did the redirection on use

The command action returned by commandSearch needs some way to know if
it is expected to return (in which case it needs to fork as it does now),
or if it is not - in which case it should exec. In current usage, all
externals are fork'd and exec'd --- whereas all others are executed in
the current process, expecting to return

### To Do ###

This is now partially fixed (as of rbe84321 )
  * shuffling of Fds for sdtin/out/err/json is now done in the child
  * appropriate closes are now done in the right place
  * [.md](.md) but all of this is only in the job code, not the main shell execution
  * [.md](.md) redirection is still done in the parent before forking
  * [.md](.md) things that should exec are still forking


# Fork, Exec, and extra processes #

When plush runs a command, it ends up with an additional process in the way
  * Because IO's execProcess does a `IO.createProcess` and a `IO.waitForProcess`
  * `ioPipeline` does something similar

Fixing this non-trivial...



# Notes #

Useful Links:
  * http://en.wikipedia.org/wiki/POSIX_terminal_interface
  * http://en.wikipedia.org/wiki/Process_group
  * Example code: http://rachid.koucha.free.fr/tech_corner/pty_pdip.html

Another method, see man page for linux's forkpty() and login\_tty()

Functions added to System.Posix.Missing (in r519f6d7 ):
  * `loginTerminal`
  * `setControllingTerminal`
  * **TODO:** offer these as a patch to base

Each launched pipeline needs to be in the foreground process group of the ptty.
The ptty needs to be the controlling terminal:
  * the shell forks
  * child gives up the controlling terminal
  * child allocates pty
  * child open pty, making it the controlling terminal
This is now done: r519f6d7

Useful commands:
```
ps -A -f -u
ps -A -O logname,sess,tty,tpgid,tsess,pgid,ppid,pid,gid,rgid,user,ruser,comm
ps -A -O tty,tpgid,pgid,ppid,pid,gid,rgid,user,ruser,comm

    logname = login name of user who started the session
    sess = session id
    tty = full name of control terminal
    tpgid = control terminal process group ID
    tses = control terminal session ID
    pgid = proess group number
    ppid = parent process id
    pid = process id
    gid = process gorup id
    rgid = real group id
    user = effective user id
    ruser = real user name
    comm = command


lsof +f g -p <pid1>,<pid2>,...

```

Code Examples:

See OpenSSH's `pty_make_controlling_tty()` in **sshpty.c** for the most comprehensive
implementation
  * it spends a lot of code giving up the old tty
  * it has code for setting the controlling tty under many unix variants


Apple's [login\_tty.c](http://www.opensource.apple.com/source/Libc/Libc-167/util.subproj/login_tty.c)
