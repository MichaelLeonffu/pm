

# Notes

Taking some notes to see what should be done as far as features go


## What files are needed for pm

- config; remember all pm settings
- memory? to remember all projects and their settings
- bin; a place to store the `pm` executable?


## Design Decisions

### Should pm be memory-less? YES

i.e if such a command as

    pm list

to list projects, should it traverse the directory structure to find the
projects or have a meta file stored?

**no**: then making edits to the directory structure would require `pm` to
  reload it's own memory

**yes**: then `pm` must scan the directory each time.

Since we want `pm` to have as little overhead as possible and not be required
to use for development i.e you don't HAVE TO use `pm` to manage your projects
then `pm` should be able to scan and understand the directory stucture...

Ofc if later we realize that this might be too difficult to do then I'll
change this choice; i.e in the case of nested directories etc.

## Notes on MACOS paths

Referenced from this [stack overflow post]
(https://stackoverflow.com/questions/603785/environment-variables-in-mac-os-x).

Macos keeps it's `PATH` var initialized someplace. There is a global one which
is the one most people use. To make a personalizied one, then a user can
create a `~/.profile` to set values including paths.

If we want to keep a universal `~/bin` for all development builds, and we want
those projects to be executable then we need to add the path to that bin.
That can be done using the `~/.profile` but not all users have that file.

## What is `~/.[shell_name]rc`

The `rc` files are for interactive shells, [reference]
(https://unix.stackexchange.com/questions/129143/what-is-the-purpose-of-bashrc-and-how-does-it-work).

Adding paths to the `~/.bashrc` can allow for us to add aliases. The suffix
`rc` means that for the `bash` shell, when run interactively, it'll execute
this code.

## Using a non bash shell

i.e if you're using `zsh` then there will be a corrisponding set of files i.e
`~/.zshrc` for the `rc` file. [Reference]
(https://unix.stackexchange.com/questions/137183/how-do-you-disable-oh-my-zsh-and-zsh-without-uninstalling-it)


This also means that to add values to an `rc` file, we need to know what shell
the user will be running. There could be an installing `pm` section for this
to initialize `pm`'s files.

## Backups exclusions

There are some cleaver ways to exclude files from backups using some scripts
But the scripts would need to be run every time. And it might be that
a backup picks up one of the files before the exclusion is performed.

[exclusion script](https://superuser.com/questions/1161038/exclude-folders-by-regex-from-time-machine-backup)

Idealy if we could regex the exlusion path though the interface of time machine
then there woudln't be such a strong need for `pm`. `pm`'s goal is to have
isolate all the src from the builds/libs/etc so that backups don't backup
a bunch of files used for projects or the resulting executables/builds

It's also for the case that one can easily clean up files that aren't being
using; this would act like a global clean. One could do this manurally by
having some sort of varaible keep track of all their projects and their clean
commands. Then run them all together with a alias or script that would run
clean for each of their projects. `pm` hopes to automate this process or
at least streamline the process so users don't have to manuall do it by hand.



