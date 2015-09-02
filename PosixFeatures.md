# Source of truth #

POSIX.1-2008 / IEEE Std 1003.1-2008 / Open Group Base Specifications [Issue 7](https://code.google.com/p/plush/issues/detail?id=7):
  * http://pubs.opengroup.org/onlinepubs/9699919799/toc.htm
  * http://pubs.opengroup.org/onlinepubs/9699919799/nframe.html




# POSIX Shell Features #

<pre>
P = Parsing implemented<br>
E = Execution implemented (Et = in test mode, Er = in real mode)<br>
D = DocTest written<br>
u = unlikely to be implemented<br>
</pre>

command lists
| PEt      | basic sequencing |
|:---------|:-----------------|
| P-       | last command w/semi-colon and w/o |
| PEt      | one command, mutliple |
| P        | sequential vs. background |

and/or lists
| P        | linebreaks after operators |
|:---------|:---------------------------|
| PEtD     | two operators              |
| PEt      | inversion of pipeline value |

pipelines
| P        | linebreaks after pipes |
|:---------|:-----------------------|

command
| P-       | assignment shuffling |
|:---------|:---------------------|
| P-       | redirect shuffling   |
| PEtD     | simple arguments     |
| P-D      | single quoted arguments |
| P-D      | backslash arguments  |
| P-D      | double quotes        |
| P-D      | backquotes           |
| ped      | $ substitutions      |

compound commands
| PED      | brace groups |
|:---------|:-------------|
| PED      | subshells    |
| PED      | for          |
| PED      | case         |
| PED      | if           |
| PED      | while / until |

features
| PED      | functions |
|:---------|:----------|
| -ED      | alias     |
| PED      | positional parameters |
| PED      | special parameters |
| PED      | variables |
| PED      | redirection |
| PED      | here documents |
| PED      | pipelines |
| -ED      | program execution |

word expansions
| PED      | tilde expansion |
|:---------|:----------------|
| PED      | parameter expansion - simple |
| PED      | parameter expansion - expression |
| PED      | parameter expansion - pattern |
| PED      | command substitution |
| PED      | arithmetic expansion |
| -ED      | field splitting |
| -ED      | pathname expansion |
| -ED      | quote removal   |

repl
|          | secondary prompts |
|:---------|:------------------|

special parameters
| PED      | @ |
|:---------|:--|
| PED      | |
| PED      | # |
| PED      | ? |
| PED      | - |
| PED      | $ |
| Ped      | ! |
| PED      | 0 |

flags
|          | allexport   -a |
|:---------|:---------------|
|          | errexit     -e |
| u        | hashall     -h |
|          | ignoreeof      |
| E        | interactive -i |
|          | monitor     -m |
|          | noclobber   -C |
| E        | noexec      -n |
|          | noglob      -f |
|          | nolog          |
|          | notify      -b |
|          | nounset     -u |
| E        | verbose     -v |
| u        | vi             |
| E        | xtrace      -x |


# POSIX Utilities #

<pre>
c = coded, partial<br>
C = coded, full<br>
D = doctests<br>
</pre>

Special Built-In Utilities -- affect shell state, var assignments affect shell
| CD   | break continue |
|:-----|:---------------|
| CD   | exit return    |
|      | exec           |
| CD   | colon .(dot) eval |
| CD   | export readonly |
| CD   | shift          |
| CD   | set            |
|      | times          |
|      | trap           |
| CD   | unset          |

Required Built-In Utilities -- "found" before path search -- affect shell state
| CD   | alias unalias |
|:-----|:--------------|
| CD   | read          |
|      | bg fg jobs kill wait |
| c    | cd fc pwd     |
|      | command getopts |
| CD   | false true    |
|      | newgrp umask  |

Likely to implement Built-Ins -- don't affect shell state
|      | basename |
|:-----|:---------|
| CD   | cat      |
|      | cmp      |
|      | cp       |
|      | date     |
|      | diff (partial) |
|      | dirname  |
| CD   | echo     |
| cd   | env      |
|      | find (partial) |
| cd   | grep (partial) |
|      | head     |
|      | ls       |
| c    | mkdir    |
|      | mv       |
|      | popd     |
|      | pushd    |
| c    | rm       |
|      | rmdir    |
|      | sed (partial) |
|      | sh       |
|      | sleep    |
|      | sort (partial) |
|      | tail     |
|      | tee      |
|      | test     |
|      | time     |
| c    | touch    |
| cd   | tr       |
|      | wc       |
|      | xargs    |