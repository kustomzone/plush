## Shell Initialization Sequence ##
  * `initialShellState` — no vars, args, funs, aliases, `defaultFlags` (all off), name is `"plush"`
  * `Plush.Run.prime`
    * `primeShellState` — import environment vars
    * `loadSummaries` — load up the summaries.txt file
  * `initialRunner` — set name, flags, args from invocation

## Files Sourced at Start Up ##

### POSIX ###
  * at start, if an interactive shell, and real user == effective user, and real group == effective group, then:
    * take ENV environment var, apply parameter expansion, and run the
    * commands in that file in the current environment
    * undefined what to do if expanded ENV isn't an absolute path

NOTE: no other expansions are performed! not tilde, command, arithmetic,
or even pathname expansion

### Bash ###

  * if interactive login shell or --login:
    * /etc/profile                                      (unless --noprofile)
    * ~/.bash\_profile <|> ~/.bash\_login <|> ~/.profile  (unless --noprofile)

  * on exit
    * ~/.bash\_logout

  * else if interactive, not login:
    * --rcfile 

&lt;file&gt;

 <|> ~/.bashrc     (unless --norc)

  * Note: .bashrc is commonly sourced from ~/.bash\_profile

  * else if non-interactive
    * if [-n "$BASH\_ENV" ](.md); then . "$BASH\_ENV"; fi

  * Note: no path search

### Bash invoked as `sh` ###

  * if interactive login shell, or --login                (and not --posix)
    * /etc/profile                                      (unless --noprofile)
    * ~/.profile                                        (unless --noprofile)

  * else if interactive
    * $ENV

  * else non-interacive
    * doesn't read anything

### Dash ###
  * if interacive login (tested because argv[0](0.md) starts with '-')
    * /etc/profile
    * ~/.profile

  * if interactive non-login, or ENV set after login processing
    * $ENV

  * Note: suggests ENV=$HOME/.shinit; export ENV  to be in ~/.profile


### Plush ###
  * if login (argv[0](0.md) stats with '-', or --login, or -w)
    * /etc/profile
    * ~/.plush/profile

  * prior to these loading, both ~/.plush/profile and ~/.profile/env are created if absent

  * if interactive (not else, and after login processing above)
    * $ENV

  * best practice is to put per-interactive things in ~/.plush/env and
  * in ~/.plush/profile set
    * `ENV=$HOME/.plush/env; export ENV`

## Code That Runs Scripts ##

  * `eval` built-in — command(s) are in a string (unclear if should be one command or multiple)
  * `dot` built-in - path searches for a script, runs it, possibly with args
  * `-c` invocation - commands are in argument string
  * file invocation - commands are in file named by argument, possibly with args (already taken care of)
  * `-s` invocation, non-interactive - commands are in stdin, possibly with args (already taken care of)
  * profile & `$ENV` - commands are in these files if present
