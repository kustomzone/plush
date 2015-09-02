# Accessing Plush Remotely #

Basic idea:
  * ssh to the remote side, running plush --remote-access.
  * The remote plush communicates back the url (or perhaps just port & key).
  * local plush adds a local port foward to the ssh connection
  * local plush constructs the url from the local port and remote key
  * local plush opens that url locally

Issues to contend with:

  * must stay interactive with the terminal the user ran plush -r on until at least credentials are entered - how can local plush distinguish between ssh's asking for credentials vs. remote plush's output?

  * If local plush/ssh combo deamonizes (ssh's -f) then how do we terminate it?
    * when the user runs exit in plush
    * when the remote plush process abnormally dies
    * when the local ssh or plush processes abnormally die
    * from the local side intentionally (HUP to ssh or plush process?)

  * picking port numbers, both local and remote

  * what set of ssh options can the user pass to plush -r? need at least host, port, & user.

  * How can all this work when double clicking a local icon? or would one do this in a local plush... for a remote plush? and then how would that work without leaving a running command?

Ways to do this

  * run ssh in ControlMaster mode, then run ssh again to add the forward after learning the port from the remote:
    * `ssh -M -S <socketlocation> <user@host> plush --remote-access`
    * `ssh -O forward -L 

&lt;localport&gt;

:localhost:

&lt;remoteport&gt;

 -S 

&lt;socketlocation&gt;

 <user@host>
    * has the difficulty of having to find a secure place for the socket
    * has the difficulty that the initial master ssh must be killed expressly - the local socket keeps it open even after the command on the master connection terminates
    * the ability to close a forwarded connection is only in ssh 6.0 - which no one has yet


# The Server #

For a user, locally, there is just one plush server. At present, it still has
just one in-process shell, but in the future it could support multiple shell
processes. This similar in architecture to both `screen` and `tmux`.

To connect to a remote host, again, one ssh connection is maintained for the
unique user@host combination. The remote plush server is contacted through a
single tunnelled connection via this ssh connection.

## Local Server ##

The local server runs daemonized (detached). When running, it maintains these
files:

```
    ~/.plush/server.json - a JSON file containing:

    {
        local: true,
        pid: <pid>,
        port: <int>,
        key: <string>,
    }

    ~/.plush/server.log - the log file
```

The server is started and managed with these plush commands:

```
    plush --local start [--port port]
    plush --local stop [--force]
    plush --local status
```

The status command does a check for the file, and if present, it then checks the
pid, and finally a ping URL request (format to be defined).

The stop command attempts to stop via a URL request (format to be defined), and
if that fails, and `--force` is present, then by killing the process. If the
processes is dead, if the file isn't removed, it removes it.

The start command will do the status logic, and if the server is running,
refuse to run.

These commands all output short human readable results. They take `--json` to
instead output results in JSON form, or `--quiet` to output no information,
setting only the exit status appropriately.

## Local Invocation ##

Normally, users don't use the `--server` commands. Instead they use the
`--local` (`-l`) command to both start the server if needed, and launch a local
web page to it.

```
    plush ( --local | -l ) [ no-launch ]
```

The code performs the status logic. If it isn't running, then the server is
started. In either event, once running, the URL is then used to launch a local
browser. However, if `--no-launch` is given, then the URL is simply printed.

The options `--quiet` and `--json` are also supported.


## Remote Invocation ##

Remote invocation essentially amounts to running

```
    plush --local --json
```

over an ssh connection, and then using that output to create a local forwarding
port, and finally opening the URL locally. The form of the remote command is:

```
    plush ( --remote | -r ) [ --no-launch ] [user@]host
```

However, remote connections want to be able to re-use ssh connections and
forwarding, and so information about such connections is kept and reused. Active
remote connections are tracked in the following files:

```
    ~/.plush/remotes/host-<user@name>.json - a JSON file containing:

    {
        destination: <user@host>,
        pid: <pid>,
        controlSocket: <file>,
        remotePort: <int>,
        localPort: <int>,
        key: <string>,
    }

    ~/.plush/remotes/control-<user@name> - the controlSocket
```

The files and connections are managed with the `--remote` commands:

```
    plush --remote start [user@]host
    plush --remote stop [user@]host
    plush --remote status [ [user@]host ]
```

These commands are analagous to the `--local` commands, only they manage the
ssh connection to the remote, rather than the plush server process.

A connection is started as follows:

  1. An ssh processes is exec'd:
    * `ssh -f -M -S <controlSocket> <user@host> plush --local --json`
    * ??? Will the plush command output make it back in light of the `-f` option?

> 2. Onces that backgrounds, establish a local port-forward to the remote port, by using the control socket:
    * `ssh -O forward -L <localport>:localhost:<remoteport> -S <controlSocket> <user@host>`
    * ??? How should it pick a localport? How does ssh report when the localport wasn't bindable?
    * ??? Does this command need to repeat the `user@host` info?

> 3. Restrict the connection from further use:
    * `ssh -O stop -S <controlSocket> <user@host>`
    * ??? Does this command need to repeat the `user@host` info?

Stopping a connection is done by these steps:

  1. Tell the connection to stop:
    * `ssh -O exit -S <controlSocket> <user@host>`
> 2. kill -HUP the pid
> 3. remove the control files if needed

The remote command, like the local command, performs the status checks, starts
a connection if needed, and finally uses the JSON info to either launch a
local browser or print out the URL.