customizepkg
============

A tool for Arch Linux package manager pacman to modify PKGBUILD automatically.

# Usage: #

Without any parameters, it will read ./PKGBUILD and show any modifications that would be done.
Create config files with the same name as the package you want to modify, and place in either:
`/etc/customizepkg.d/` or `~/.customizepkg/`. You can export the environment
variable `$CUSTOMIZEPKG_CONFIG` to change the directory customizepkg will look
for the config files in.

The pacman wrapper "Yaourt" integrates with customizepkg by default

# Configuration file Syntax: #

```
ACTION#CONTEXT#PATTERN#VALUE
```

- Action can be: remove, removeline, add or replace
- Context can be: depends, conflicts, makedepends etc.. or global for matching regexp in the whole PKGBUILD
- Pattern can be any rexgexp
- Value (only needed for replace) can be any string
- Comments can be added by starting the line with `#`
- The seperator can be changed by adding "@SEPARATOR=" to a configfile

# Additional Files #

You can add extra files to a PKGBUILD by placing them in `/etc/customizepkg.d/$PACKAGENAME.files/`
They will be added automatically, including checksums

# Run a script #

If the configuration file has the executable flag set, it will be executed instead of parsed.
Two parameters are passed to the script:

- $1 is the original PKGBUILD
- $2 is the custom PKGBUILD

# Patch the source #

Patch files can be applied to the source code using the following syntax:

```
patch#1#file.patch
```

- The context ("1" above) is the number of leading components to strip (i.e. `patch -p1`)
- The path cannot contain variables
- The patch file can be stored in the additional files directory, $PACKAGENAME.files
