# xdg-utils Dependencies

This document aims to guide package maintainers and developers through the dependencies relied on by the xdg-utils. For ease of maintaining it it will list the command names instead of the package names.

## Status

* `xdg-desktop-icon` last evaluated at `0f6385262`
* `xdg-desktop-menu` last evaluated at `0f6385262`
* `xdg-email` last evaluated at `0f6385262`
* `xdg-icon-resource` last evaluated at `0f6385262`
* `xdg-mime` last evaluated at `0f6385262`
* `xdg-open` last evaluated at `0f6385262`
* `xdg-screensaver` last evaluated at `0f6385262`
* `xdg-settings` last evaluated at `0f6385262`

## Build dependencies

To successfully build the xdg-utils using `./configure` and `make` the following dependencies are needed:

* `make`
* `xmlto` for bulding the documentation
* One of `lynx`, `w3m` or `links` to render the documentation to plain text

If you are a developer please also install `shellcheck` to be able to use `scripts/shellcheck-xdg-util.sh`.

## Always Expected dependencies

All xdg-utils rely on a POSIX compliant shell being available via `/bin/sh`.

Always required are the [POSIX commands marked as Mandatory](https://en.wikipedia.org/wiki/List_of_POSIX_commands), not all of them are used but developers are safe to assume that they exist.

A `realpath` implementation is also always expected.

Other required commands are:
* `dbus-send` for interacting with with the DBus
* `echo -e` for xdg-email
* `mimetype` or `file` for xdg-mime
* `hostname` or `uname -n` for xdg-open
* `hostname` for xdg-screensaver
* `ps -p` for xdg-screensaver


## Optional dependencies

* `gtk-update-icon-cache` (xdg-icon-resource)
* `mimeopen` (xdg-open)
* `mv -T` (xdg-screensaver)
* `run-mailcap` (xdg-open)
* `update-desktop-database` (xdg-desktop-menu)
* `update-mime-database` (xdg-mime)
* `xautolock` (xdg-screensaver)
* `xdg-user-dir` (xdg-desktop-icon)

DBus Services:
* `org.freedesktop.ScreenSaver` (xdg-screensaver)

## Commands required for integration

### X11

* `xprop` (xdg-screensaver)
* `xscreensaver-command` (xdg-screensaver)
* `xset` (xdg-screensaver)

### Windows WSL
* `explorer.exe` (Detecting WSL, xdg-open)
* `rundll32.exe url.dll,FileProtocolHandler`

### Windows Cygwin
* `cygstart` (xdg-open)

### KDE

for KDE 6:
* `kbuildsycoca6` (xdg-mime)
* `kde-open` (xdg-open)
* `kmimetypefinder` (xdg-mime)
* `kreadconfig6` (xdg-settings, xdg-email)
* `kwriteconfig6` (xdg-settings)
* `qtpaths` will be removed with !120 (xdg-mime)

for KDE 5:
* `kbuildsycoca5` (xdg-mime)
* `kde-open5` (xdg-open, xdg-email)
* `kmimetypefinder5` (xdg-mime)
* `kreadconfig5` (xdg-settings, xdg-email)
* `ktraderclient5` (xdg-mime)
* `kwriteconfig5` (xdg-settings)
* `qtpaths` (xdg-mime, will be removed with !120)

for KDE 4:
* `kbuildsycoca4` (xdg-mime)
* `kde4-config` (xdg-mime, xdg-settings, xdg-icon-resource, xdg-desktop-icon)
* `kde-open` (xdg-open, xdg-email)
* `kmimetypefinder` (xdg-mime)
* `kreadconfig` (xdg-settings, xdg-email)
* `ktraderclient` (xdg-mime)
* `kwriteconfig` (xdg-settings)

for KDE 3:
* `dcop` (xdg-screensaver)
* `kbuildsycoca` (xdg-mime)
* `kde-config` (xdg-mime, xdg-settings, xdg-icon-resource, xdg-desktop-icon)
* `kfile` (xdg-mime)
* `kfmclient` (xdg-open)
* `kreadconfig` (xdg-settings, xdg-email)
* `ktradertest` (xdg-mime, xdg-settings)
* `kwriteconfig` (xdg-settings)

### Deepin
* `dde-open` (xdg-open optional)
* `jq` (xdg-settings)

### Gnome 2

* `gconftool-2` (xdg-settings, xdg-desktop-icon)
* `gio` (xdg-open optional)
* `gnomevfs-info` (xdg-mime optional)
* `gvfs-info` (xdg-mime optional)
* `gvfs-open` (xdg-open optional)
* `perl` with `Net::DBus` and `X11Protocol` (xdg-screensaver)

Required DBus Services:
* `org.gnome.ScreenSaver` (xdg-screensaver)

### Gnome 3+

* `gio` (xdg-open optional)
* `gnome-open` (xdg-open optional)
* `gnomevfs-info` (xdg-mime optional)
* `gvfs-info` (xdg-mime optional)
* `gvfs-open` (xdg-open optional)

### Mate

* `gio` (xdg-open optional)
* `gvfs-open` (xdg-open optional)
* `mate-open` (xdg-open optional)
* `mate-screensaver-command` (xdg-screensaver)

Required DBus Services:
* `org.mate.ScreenSaver` (xdg-screensaver)

# Cinnamon

Required DBus Services:
* `org.cinnamon.ScreenSaver` (xdg-screensaver)

### Xfce

* `exo-open` (xdg-open optional)
* `gio` (xdg-open optional)
* `gvfs-open` (xdg-open optional)

### Lxqt

* `qtxdg-mat` (xdg-mime, xdg-settings)

### Enlightenment

* `enlightenment_open` (xdg-open optional)

### Flatpak

* `gdbus` (xdg-open)

Required DBus Services:
* `org.freedesktop.portal.Desktop`

## A note on `realpath`

The xdg-utils support multiple styles of realpath implementations:
* `realpath -- <file>` (GNU-style)
* `realpath <file>` (busybox-style)
* `readlink -f <file>` (fallback in case there is no realpath)

The xdg-utils can automatically detect which kind of realpath is present. If none can be found `readlink -f` is expected to be the symlink reading version, without checking.

Since the 2024 POSIX version there is a [standardised realpath](https://pubs.opengroup.org/onlinepubs/9799919799/), but the xdg-utils will support the other implementations to work on older systems.

For developers: You want to use `xdg_realpath <file>` inside the xdg-utils.

## For developers

Please avoid GNU-isms! The xdg-utils are supposed to run in a wide variety of environments which don't implement GNU-extensions.

Environments to consider:
* GNU
* Busybox
* FreeBSD / NetBSD / OpenBSD
* Mac OS
* Probably others too …

### On awk

When writing `awk` code, which should be the preferred language if more advanced data processing is needed please test your code with `mawk` or, if you have the gnu version test with `awk --posix`.

See:
* [The POSIX awk documentation](https://pubs.opengroup.org/onlinepubs/9799919799/)
* [the mawk documentation](https://invisible-island.net/mawk/manpage/mawk.html)
