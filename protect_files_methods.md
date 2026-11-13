# Methods to Protect Files

When you delete a file in Linux with `rm` in the command line, it doesn't send it to the recycle bin.

Instead the file is 'unlinked' and the data it took up is freed for future use. In other words, when you delete a file, it's gone.

While this can be catastrophic for important files, it can be overcome with some safety measures:

## Permanent Aliasing in the .bashrc file

`rm` has a number of options and `-i` allows you to be prompted each time:

```
rm -i file

> rm: remove regular empty file 'file'?

> y
```

Now the file is deleted, and you had the chance to enter 'n', 'Enter', or anything other than string starting with 'y' to change your mind.

Aliasing `rm` to `rm -i` can be done temporarily (for your terminal session) with:

```
alias rm='rm -i'
```

This can be made permanent by adding the alias to your `~/.bashrc` file.

Pop that same command at the bottom of the file, and now every new terminal window will load your personal settings (including this alias).

### Pros/Cons

* An aliased `rf` can get annoying and potentially confusing when we want to purge lots of files (say with `rm -r` (recursive removal), or delete files after testing.
* Interestingly, `rm -rf` (recursively delete, force) will unpack to `rm -i -rf` and with `f` coming last it will override the prompt. My question would be: did you expect that behaviour, and have you been lulled into a false sense of security because of the alias?
* The other point I would raise is this: the Linux system is designed to do what you tell it to. Would it not make more sense to protect the files rather than alter the behaviour of shell commands?

## Trash CLI - a 'Recycle Bin' style utility

There is a command line utility called 'trash-cli' that behaves similarly to a Recycle Bin.

This can be installed using your package manager (i.e. `sudo pacman -S trash-cli`)

Instead of `rm` on files you can use `trash`.

You could options to restore items from trash (to their original directory), empty the trash after X days, etc.

It is therefore pretty similar to the Windows style Recycle Bin.

### Pros/Cons

* This is good for users who want the functionality of the Recycle Bin they may have been used to.
* However it means `rm` carries the same risk in deleting important files. It doesn't make `rm` safe, it just adds another option for deletion.
* `trash` is another command for the user to remember (or of course not remember)

## chattr (change attributes)

`chattr` is available as a core utility (you can use it on most Linux distributions out of the box) and allows you to protect files from both overwrites and deletion.

```
chattr +i <file> # make immutable

chattr +a <file> # make it so you can only append (add to) rather than edit in line
```

### Pros/Cons

* this is not viable for files you intend to edit, even with the 'append only' option is also available

## Amend Directory Permissions

Make the parent directory owned by root without write permissions for group and owner

```
sudo chown root:root /path/to/directory

sudo chmod 755 /path/to/folder # only root can 'write' directory i.e. delete files

sudo chown <user>:<user> /path/to/directory/file

sudo chmod 644 /path/to/directory/file
```

### Pros/Cons

* High number of commands to run
* Flexibility for recursive option where all are important (say Pictures) or one special file where the others are not so essential (e.g. Documents/retirement_plan)

## The final option - Just be careful

Having a good degree of care with what you delete and what you truncate (`>`) in Linux will always be a good idea.

Failing that, one case of an accidental deletion or truncation may be all you need to never make that same mistake twice - this was the case with me.

If you can take care and backup your files, you are unlikely to face the prospect of total data loss.
