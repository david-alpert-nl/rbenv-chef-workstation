# rbenv-cinc-workstation: Use cinc Workstation with rbenv

This plugin lets you treat [cinc Workstation](https://downloads.cinc.io/cinc-workstation/) as another version in [rbenv](http://rbenv.org/).

cinc Workstation replaces cincDK. If you want to use cincDK instead, then use the old [rbenv-cincdk plugin](https://github.com/docwhat/rbenv-cincdk) instead.

## Requirements

-   [cinc Workstation](https://downloads.cinc.io/cinc-workstation/) installed in `/opt/cinc-workstation`

## Installation

To install rbenv-cinc-workstation, clone this repository into the `$(rbenv root)/plugins` directory.

```sh
$ cd $(rbenv root)/plugins
$ git clone <url>
```

**Warning:** If you've previously installed cinc Workstation, you will want to undo the changes recommended in the [cinc Workstation install instructions](https://docs.cinc.io/install_dk.html#set-system-ruby) or in the Homebrew info gist. Otherwise `rbenv` or cinc Workstation will not work correctly.

Then create an empty directory in `$(rbenv root)/versions` called `cinc-workstation`:

```sh
$ mkdir "$(rbenv root)/versions/cinc-workstation"
```

Finally, change to the new `cinc-workstation` version and run `rbenv rehash`.

```sh
$ rbenv shell cinc-workstation
$ rbenv rehash
$ rbenv which ruby
/opt/cinc-workstation/embedded/bin/ruby
```

That's it!

## Troubleshooting

If you are having problems, try running `sanity-check.sh`:

```sh
$ $SHELL "$(rbenv root)/plugins/rbenv-cinc-workstation/sanity-check.sh"
```

### Error running commands and errors talk about cincdk

Make sure you have completely uninstalled cincdk:

```sh
# This removes old Gems and the rbenv-cincdk plugin.
$ rm -rf ~/.cincdk "$(rbenv root)/plugins/rbenv-cincdk" "$(rbenv root)/versions/cincdk"
```

## Frequently Asked Questions

### Hey, what happened to `gem`?

With the cinc Workstation you have to use `cinc gem` instead.

### Why don't you include `/opt/cinc-workstation/embedded/bin`?

We don't fully include the `/opt/cinc-workstation/embedded/bin` directory because it'll break your system in subtle ways.

`/opt/cinc-workstation/embedded/bin` has commands like `clear`, `tput`, `xsltproc`, and `xz`. These are commands that are part of your system and are only included in cinc Workstation so it gets reliable results across platforms.

If we made shims of these commands, your system might break when not using cinc-workstation and can produce unexpected results even if you were using cinc-workstation if your version of these commands are different (e.g. Gnu vs. BSD or version).

### Why not just symlink `/opt/cinc-workstation/embedded` to `$(rbenv root)/versions/cinc-workstation`?

For the same reason we don't include `/opt/cinc-workstation/embedded/bin`: It breaks systems commands in unexpected ways.

### Rbenv isn't working or is using gems from the wrong place

Make sure you undid any changes recommended by the [cinc Workstation install instructions](https://docs.cinc.io/install_dk.html#set-system-ruby) or in the Homebrew info gist.

Specifically, make sure you aren't calling `cinc shell-init` anyplace in your shell startup files.

### "can't find executable cinc (Gem::Exception)"

This is because you are using [`rbenv-bundle-exec`](https://github.com/maljub01/rbenv-bundle-exec).

You'll have to tell `rbenv-bundle-exec` to ignore a bunch of binaries that are only in cinc Workstation:

```sh
echo cinc >> ~/.no_bundle_exec
echo berks >> ~/.no_bundle_exec
```

## Questions?

You can reach me at [docwhat.org](https://docwhat.org/email/) or as docwhat on [Freenode IRC](https://freenode.net/).

## License

See the `LICENSE` file

## Note on Patches/Pull Requests

-   Fork the project.
-   Make your feature addition or bug fix.
-   Add tests for it. This is important so I don't break it in a future version unintentionally.
-   Commit
-   Send me a pull request. Bonus points for topic branches.
