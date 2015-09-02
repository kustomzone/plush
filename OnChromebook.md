# Running plush in a crouton chroot on a Chromebook #

**Goal:** Have a plush terminal running in in ChromeOS that access the chroot environment provided by Crouton.

## Prereqs. ##

1. Install Crouton on your Chromebook
> https://github.com/dnschneid/crouton

2. Install HaskellPlatform & Plush:
> http://www.haskell.org/platform/

> https://code.google.com/p/plush/

3. You should now have plush on your path. This should work:
```
    plush --version
```

## Linking your plush environment to Chrome OS ##

1. Run
```
    plush -l no-launch > ~/Downloads/plush.txt
```

Plush requires a URL with a token generated at launch to install to connect. We are directing that token into the shared `~/Downloads` directory setup by Crouton.

2. Change back into Chrome OS.

3. Open a terminal tab. `[CTRL-ALT-T]`, "shell", then:
```
    cat ~/Downloads/plush.txt
```

4. Copy paste the URL to the Chrome URL window.

5. Enjoy your comfy shell.

_— recipe by Zaki Manian_