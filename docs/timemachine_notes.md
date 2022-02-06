
# Time Machine

*The ideal way to backup on mac*

Time Machine is the most convenient and efficient way to back up on macos. It
comes pre-installed, low overhead, very fast, automatically purges old
backups, and works like you would expect it to...

...until you try to take a deeper look at it and get super confused.

Specifically if you `Enter Time Machine` and you start to see files and
folders that you thought you excluded. i.e I exclude my `~/Downloads` because
they can be a large temp files, but when I check my Time Machine the files
may still be there. Also another issue is: "What happens if I add a symbolic
link or an alias?" Would Time Machine follow those as well and back those up?
How can we know what Time Machine is backing up or not backing up.

## Excluding files/folders

In the Time Machine preferences, click on `Options...` to see which
files/folders are being excluded. You can add more files/folders to the list
using the `+` button.

Since you know know when Time Machine creates it's backups -- **it can take
snapshots even if your Time Machine disk isn't connected** -- I recommend
first you make the directory you want to exclude, before moving files to it.
i.e make the folder `~/exclude_me` before moving your `very_large_file.img`
into it

This way you wont be caught by a snapshot taking place. e.g if you moved the
`very_large_file.img` into that folder and it took a snapshot then you would
have a `ver_large_file.img` in your backups. And it's not easy to remove
files from the backups once it's backed up.

## What to exclude from Time Machine to save space

**I'll update this if I find out, check out the readings at the end.**

## Removing files from the backups

**I don't know how to do this yet**

## Symbolic link, Hard links, Aliases

In order to understand how Time Machine treats shortcuts, first lets review
all the different shortcuts we can make in macos.

In macos, which follows UNIX, there is naturally symbolic links and hard
links.

- Symbolic link is like a shortcut; you can click on the shortcut to get to
  the file/folder you shortcut to. Symbolic links remember the path to a
  file, moving the file/folder would "break" the symbolic link, the link
  would take you to where the file/folder used to be.
- Hard link is like a file itself; it's as if you have two files except it's
  only one copy; it's like having 2 doors to the same building, you can
  delete a door but as long as one door remains then the file still exists.
- Aliases, macos's own type of shortcuts, keeps track of where the file is at
  all times. If you move the file then the alias still works. It keeps track
  of the file/folder itself.

Each have their own pros and cons:
- Symbolic links: you can replace the file/folder with another file/folder of
  the same name and the symbolic link will point to the new file/folder. An
  Alias would not do that since it'll track where the original file was moved
  to.
- Aliases are easier to make than the other two which requires terminal
  commands: `ln` and `ln -s`.
- Hard links: only work on files and not folders.

Each one has a slightly different effect on Time Machine

## Time Machine and shortcuts

*Does Time Machine follows shortcuts?*

**No.** *Unless it's a hard link*

I'll explain case by case:

### Symbolic link and Time Machine

Since a symbolic link is just a path, Time Machine will save the "symbolic
link file" but if the destination doesn't exist, i.e it's excluded, then the
symbolic link shortcut doesn't do anything. It's just a path and the path
points to nothingness.

You can test this by making a symbolic link and checking to see that it fails
to find the file it's linking to if the file is an excluded one.

### Aliases and Time Machine

Very tricky, since alias tries to keep track of where the original file is,
when you use it in your backups, it will find the most recent version of the
existing file. I.e in the morning I created a file `big_data.img` in my
`~/excluded/` folder. I make an alias to it called `big_alias` and at night I
exclude my `~/excluded/`folder. Sometime in the afternoon a snapshot was
saved. If I use the `big_alias` in Time Machine, it'll take me to
`big_data.img` in the `~/excluded/` even though my backup for `~/excluded/`
doesn't exist at night. What's going on is that it's going to the backup from
the morning.

It can also refer to your computer's copy of the file/folder. It's very tricky
since it can go forwards in time or back in time to find the file it's
looking for.

I tired testing this by creating and removing files at different times, and
creating backups between each change. This allowed me to know what is in the
far past, recent past, and present and where the alias points to.

### Hard links and Time Machine

Since hard links are like two doors to the same building. Excluding one door,
you can say this is the original folder the file lives in, is actually not
enough to exclude it from Time Machine. This is because Time Machine only
looks for the doors to buildings, but doesn't check if there are any excluded
paths leading to other doors to that same building. i.e If Time Machine finds
the front door to a building (file), it'll back it up. It doesn't care if the
side door is an excluded file or in an excluded path.

You can test this by making a file in an excluded path, then making a hard
link to that file. The file will remain in the Time Machine because it really
thinks that is the file. (For those who know what hard links are this should
be really easy to understand, since hard links are really the file in
question.)

## How to actually read Time Machine

I'll answer the following questions:
- "Why does Time Machine show my excluded files/folders?"
- "Why do excluded files show up and disappear in Time Machine?"

First things first open a finder and press `cmd + alt + p`. This will show you
the path to the folder or file you're looking at.

To answer the questions, first realize that the second question gives us some
information. "files show up and disappear in Time Machine", if this is the
case for you then `Menu > Time Machine > Enter Time Machine` and in your time
machine look at the path at the bottom of the finder, you should see either:

1. Macintosh/Users/username/path... 2. data@snap/Users/username/path... 3.
TimeMachine/backup/data/Users/username/path...

The first one is your current system. You're looking at your own finder and
HD. This is **not** a backup its your live files.

The second one **is a** backup but it's not on the external backup drive that
you have. This is a snapshot taken of your system with the excluded files
included. Don't worry these files are excluded when they reach the backup
disk.

The last case **is also a** backup, this is the one that is on your external
backup disk. You should not see excluded files here since you excluded them.

When navigating through Time Machine you start in case 1: your own system. If
you click on a back up in the right hand side, you'll see one of the two
following cases. You can tell them apart by looking at the paths. The second
case clearly states that it's in your system, whereas the third case shows
that the backup is on the external backup disk.

So to answer the questions:

    Why does Time Machine show my excluded files/folders?

Because you're looking at either your live system, or your local snapshot
backup before it's moved onto the external backup disk.

    Why do excluded files show up and disappear in Time Machine?

This is because you're alternating between case 3, external backups, and one
of the two other cases.

## Takeaways

- Excluding works, just make sure to know what you're looking at.
- Make sure to exclude before you move files into that excluded folder.
- Aliases are tricky, try to understand them and trust in the system.
- Soft links (symbolic/alias) aren't follows but hard links are.
- Make a soft link to a file you want to use live but don't want to back up.
  This can be like lib files for programmers. Make sure to exclude the path
  to the lib files so they're not backed up.

## Further readings

- [Saving space on Time Machine]
  (https://www.howtogeek.com/294600/save-space-on-your-time-machine-drive-by-excluding-these-folders-from-backups/)
- [Exclusion debugging]
  (https://apple.stackexchange.com/questions/405071/time-machine-is-not-excluding-downloads)
