
# pm

__Project Manager__

A tool used to help organizing a developer's development directories.

**Very unrefined README, pulling qoutes from discord**

## Features

Maybe i could do some magic with symbolic links...  need to test how this
works in terms of backups. But I'm also fine with writing a script which can
generate a project directory stucture and any configs needed for the IDEs I
use;

-----

If that script can be written then it can also in the future do something
like

`>pm --open annalibot`

and it would open my annalibot IDE workspace, terminal, and finder; activate
VENVs. It can also set aliases so that later one could do something like

`>pmbuild` which is an aliased command configured byt the opened project But
if you have more than one project open you could also just name the project i
guess?

-----

`>pm --new friends/ryan`

to make a project in the directory of friends and sub dir of ryan. <-- though
when it comes to this I'll likely do this by hand bc scripts usually are
misc.

some keywords would have be to ommited i.e `misc` or maybe just using
`_misc` ? not sure about this feature yet; can be configured for sure

-----

`>pm --clean all`

would clean all libs; this saves space.

-----

`>pm --setup`

this sets up the whole `pm` software, including it's own storage for keeping
track of projects, setting ENVs and other aliases one can use.

-----

fun features would be like

`>pm --recent`

opens most recent project

## Philosophy

But as you can see the two highlighted things are problematic. While one could
run "clean" or "make clean" that doens't solve the whole problem.

Goals:
- **Each project have the same dir sructure with everything included as
    above**
- **easy to use struture and to set up or take down**
- **all builds/libs/venvs in a known location to prevent it on showing up in
    backups or to easily remove in the future**
- **the builds/libs/venvs for each project need to be in the same location**

Rules:
- **main src/git project folder is needed for src control**
- **overarching project directory is needed for organization**
- **misc files go to misc i.e images/videos/documents/artifacts NOT CONFIGS**
- **must be easy to set up/ take down** i.e i don't want to configure my IDE
    or be stuck on only 1 IDE while using this method


## Implementation ideas

I've thought about some top down features to build for `pm` but when it comes
to making `pm` I think starting bottom up is better. I'll be looking at how
to get python and npm to work with `pm` i.e what commands are nessasary.

That would give me a better understanding of what is needed of `pm`. Later
I'll work on getting IDEs to work with `pm` and that will be more difficult.

i'll prob start working on this from bottom up; meaning: make a python or npm
project; then see how to use their api in order to know what commands I need
i.e where to put the npm modules, where to put the python modules (venv); how
to venv into a python project. how to tell hpm where to build to; how to tell
python where to build to (pyc) files

## etc

- `project/src` all the src
- `project/lib` all the libs
- `project/bin` all the builds
- `project/etc` all the configs

(https://askubuntu.com/questions/13996/where-should-i-place-source-code-that-i-wish-to-compile)
but it's really the same thing I have up there anyways. i want things to work
out of the box. Things like npm modules would need to be configured, so that
they build outside of where they normally would. This might be easy this
might not be. maybe there is a global way of setting that. Also for things
like VENVs they need to all be grouped as well.

Eventually all project's builds, venvs, libs need to be grouped
