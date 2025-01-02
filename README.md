# NOTE

This is the mh fork of arch's xorg-server package. My changes are mostly for my
use-case only.

The master branch is not where most of the action occurs, instead, look for the
relevant version you want as a tag, then see if an equivalent `mh-*` version
exists as well. For example: version 21.1.13-1 has mh-21.1.13-1 as its mh
equivalent. Thoses forks are kept in the `pkg` branch.

## Changes

### [`nosoy.patch`](./nosoy.patch)

1. remove `systemd`/`dbus` init/fini routine calls from
   `hw/xfree86/common/xf86Init.c`: this prevents xorg from logging out every
   time it tries to connect to an unexistent `dbus` system bus
