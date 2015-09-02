This is information about the command line options for plush itself.
It is compliant with the POSIX spec for the command line of `sh`.

## Single Letter Options ##

| ∓a | ƒ allexport                |  A |  |
|:---|:---------------------------|:---|:-|
| ∓b | ƒ notify (background)      |  B |  |
| -c | ☆ commands in 1st arg      | ∓C | ƒ noclobber |
|  d |                            |  D |  |
| ∓e | ƒ errexit                  | ∓E | (reserved for emacs flag) |
| ∓f | ƒ noglob                   |  F |  |
|  g |                            |  G |  |
| ∓h | ƒ cache command loc.       |  H |  |
| -i | ƒ interactive              |  I |  |
|  j |                            |  J |  |
|  k |                            |  K |  |
| -l | ★ local                    |  L |  |
| ∓m | ƒ monitor (jobs)           |  M |  |
| ∓n | ƒ noexec                   |  N |  |
| ±o | ƒ write/set options        |  O |  |
| -p | port                       | ∓P | ƒ parseout |
|  q |                            |  Q |  |
| -r | ★ remote                   |  R |  |
| -s | ☆ commands from stdin      |  S |  |
|  t |                            |  T |  |
| ∓u | ƒ unset (gives err)        |  U |  |
| ∓v | ƒ verbose (echo in→out)    | ∓V | (reserved for vi flag) |
| -w | (deprecated)               | -W | (reserved) |
| ∓x | ƒ xtrace (echo exp.)       |  X |  |
|  y |                            |  Y |  |
|  z |                            |  Z |  |

Those marked ☆ or ★ (star) are modes, and are mutually exclusive. Those marked ★ are non-POSIX.

Those marked ƒ are flags, defined by the `set` command.

  * They can be enabled (-) or disabled (+).
  * The option `o` has differnt meanings for `+` and `-` variants, and another if it is followed by an optional operand argument.
  * The option `i` isn't technically a `set` flag, but is implemented as one in Plush. It's behavior is POSIX compliant.
  * THe option `P` is a non-POSIX flag that acts like `v`, but echos the parse tree, not the input. It is mainly intended for debugging Plush itself.

**Note** that single letter mode options must not conflict with the `set` flag
options. This is because the absence of a mode option is treated a POSIX
mode, and can have flag options.

### POSIX modes ###

```
    plush                                   # comamnds from stdin
    plush command_file [argument...]        # commands from command_file
    plush -c command_string [command_name [argument...]]
                                            # commands in first arg
    plush -s [argument...]                  # commands from stdin
```

These accept any of the `set` flag options.

### Web modes ###

```
    plush -l                # local web server & browser page
    plush -r host           # remote connection
```

See RemoteFeatures for additional options.

### Testing Modes ###

```
    plush --doctest [-v] [--tag tag] [file...]                   # run doctests
    plush --shelltest [-v] [--tag tag] shell_command [file...]   # run doctests on a shell
```

`-v` sets verbose mode. When used with these modes, it is not a `set` flag.

`--tag` sets the tag to use when deciding tests to skip or include. If absent, it is inferred from the shell name.


### Universal Options ###

Either `-` and `--` end options and signal operands.

`--testexec` runs the shell in the `TestExec` monad, rather than `IO`



