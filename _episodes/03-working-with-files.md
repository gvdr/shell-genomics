---
title: "Working with Files"
teaching: 0
exercises: 0
questions:
- "Key question"
objectives:
- Use the wild card shortcut
- Use the history command to view recently used commands
- View, copy, move, and create files
keypoints:
- "First key point."
---

## Working with Files

### Our data set: FASTQ files

Now that we know how to navigate around our directory structure, lets
start working with our sequencing files. We did a sequencing experiment and 
have two results files, which are stored in our `untrimmed_fastq` directory. 

### Wild cards

Navigate to your `untrimmed_fastq` directory.

~~~
$ cd ~/dc_sample_data/untrimmed_fastq
~~~
{: .bash}

We are interested in looking at the FASTQ files in this directory. We can list
all files with the .fastq extension using the command:

~~~
$ ls *.fastq
~~~
{: .bash}

~~~
SRR097977.fastq  SRR098026.fastq
~~~
{: .output}

The `*` character is a wildcard character which stands for "everything". 
Thus, `*.fastq` matches every file that ends with `fastq`. 

This command: 

~~~
$ ls *977.fastq
~~~
{: .bash}

~~~
SRR097977.fastq
~~~
{: .output}

lists only the file that ends with `977.fastq`.

We can use the command `echo` to see how the wildcard character is intepreted by the
shell.

~~~
$ echo *.fastq
~~~
{: .bash}

~~~
SRR097977.fastq SRR098026.fastq
~~~
{: .output}

The `*` is expanded to include any file that ends with `.fastq`.

This command:

~~~
$ ls /usr/bin/*.sh
~~~
{: .bash}

Lists every file in `/usr/bin` that ends in the characters `.sh`.

> ## Exercise
> Do each of the following tasks from your current directory using a single
> `ls` command for each.
> 
> 1.  List all of the files in `/usr/bin` that start with the letter 'c'.
> 2.  List all of the files in `/usr/bin` that contain the letter 'a'. 
> 3.  List all of the files in `/usr/bin` that end with the letter 'o'.
>
> Bonus: List all of the files in 'usr/bin' that contain the letter 'a' or the
> letter 'c'.
> 
> Hint: The bonus question requires a Unix wildcard that we haven't talked about
> yet. Trying searching the internet for information about Unix wildcards to find
> what you need to solve the bonus problem.
> 
> > ## Solution
> > 1. ls /usr/bin/c*
> > 2. ls /usr/bin/*a*
> > 3. ls /usr/bin/*o
> > Bonus: 
> > 
> {: .solution}
{: .challenge}


## Command History

If you want to repeat a command that you've run recently, you can access previous
commands using the up arrow on your keyboard to go back to the most recent
command. Likewise, the down arrow takes you forward in the command history.

A few more useful shortcuts: 

`^-C` (Control-C) will cancel the command you are writing, and give you a 
fresh prompt.

`^-R` (Control-R) will do a reverse-search through your command history.  This
is very useful.

`^-L` (Control-L) or the `clear` command will clear your screen.

You can also review your recent commands with the `history` command, by entering:

~~~
$ history
~~~
{: .bash}

to see a numbered list of recent commands. You can reuse one of these commands
directly by referring to the number of that command.

For example, if your history looked like this:

~~~
259  ls *
260  ls /usr/bin/*.sh
261  ls *R1*fastq
~~~
{: .output}

then you could repeat command #260 by entering:

~~~
$ !260
~~~
{: .bash}

Type `!` (exclamation point) and then the number of the command from your history.
You will be glad you learned this when you need to re-run very complicated commands.

> ## Exercise
> Find the line number in your history for the command that listed all the .sh
> files in `/usr/bin`. Rerun that command.
{: .challenge}


## Examining Files

We now know how to switch directories, run programs, and look at the
contents of directories, but how do we look at the contents of files?

One way to examine a file is to print out all of the
contents using the program `cat`. 

Enter the following command from within the `untrimmed_fastq` directory: 

~~~
$ cat SRR098026.fastq
~~~
{: .bash}

This will print out all of the contents of the `SRR098026.fastq` to the screen.


> ## Exercise
> 
> 1. Print out the contents of the `~/dc_sample_data/untrimmed_fastq/SRR097977.fastq` file. What is the last line of the file? 
> 2.  From your home directory, and without changing directories,
> use one short command to print the contents of all of the files in
> the `~/dc_sample_data/untrimmed_fastq` directory.
> 
> > ## Solution
> > 1. The last line of the file is `TATTTTAAAATGGAATACCTAACATGTTAATTAACC`.
> > 2. cat ~/dc_sample_data/untrimmed_fastq/*
> {: .solution}
{: .challenge}

`cat` is a terrific program, but when the file is really big, it can
be annoying to use. The program, `less`, is useful for this
case. `less` opens the file as read only, and lets you navigate through it. The navigation commands
are identical to the `man` program.

Enter the following command:

~~~
$ less SRR098026.fastq
~~~
{: .bash}

Some navigation commands in `less`

| key     | action |
| ------- | ---------- |
| "space" | to go forward |
|  "b"    | to go backward |
|  "g"    | to go to the beginning |
|  "G"    | to go to the end |
|  "q"    | to quit |

`less` also gives you a way of searching through files. Use the
"/" key to begin a search. Enter the word you would like
to search for and press `enter`. The screen will jump to the next location where
that word is found. 

**Shortcut:** If you hit "/" then "enter", `less` will  repeat
the previous search. `less` searches from the current location and
works its way forward. Note, if you are at the end of the file and search
for the sequence "CAA", `less` will not find it. You need to go to the
beginning of the file and search.

For instance, let's search the file we have open for the sequence `GTGCGGGCAATTAACAGGGGTTCAC`.
You can see that we go right to that sequence and can see
what it looks like.

> ## Exercise
>
> What are the next three nucleotides (characters) after the sequence quoted above?
> 
> > ## Solution
> > `TCA`
> {: .solution}
{: .challenge}

Remember, the `man` program actually uses `less` internally and
therefore uses the same commands, so you can search documentation
using "/" as well!

There's another way that we can look at files, and in this case, just
look at part of them. This can be particularly useful if we just want
to see the beginning or end of the file, or see how it's formatted.

The commands are `head` and `tail` and they let you look at
the beginning and end of a file, respectively.

~~~
$ head SRR098026.fastq
$ tail SRR098026.fastq
~~~
{: .bash}

The `-n` option to either of these commands can be used to print the
first or last `n` lines of a file. To print the first/last line of the
file use:

~~~
$ head -n 1 SRR098026.fastq
$ tail -n 1 SRR098026.fastq
~~~
{: .bash}

## Creating, moving, copying, and removing

Now we can move around in the file structure, look at files, search files, and
redirect output to new files or to other commands. But what if we want to copy files or move
them around or get rid of them. Most of the time, you can do these sorts of file manipulations without the command line,
but there will be some cases (like when you're working with a remote computer like we are for this lesson) where it will be
impossible. You'll also find that you may be working with hundreds of files and want to do similar manipulations to all 
of those files. In cases like this, it's much faster to do these operations at the command line.

### Copying Files

When working with computational data, it's important to keep a safe copy of that data that can't be accidentally overwritten or deleted. 
For this lesson, our raw data is our FASTQ files.  We don't want to accidentally change the original files, so we'll make a copy of them
and change the file permissions so that we can read from, but not write to, the files.

First, let's make a copy of one of our FASTQ files using the `cp` command. 

Navigate to the `data` directory and enter:

~~~
$ cp SRR098026.fastq SRR098026-copy.fastq
$ ls -F
~~~
{: .bash}

~~~
SRR097977.fastq  SRR098026-copy.fastq  SRR098026.fastq
~~~
{: .output}

We now have two copies of the SRR098026.fastq file, one of them named SRR098026-copy.fastq. We'll move this file to a new directory
called `backup` where we'll store our backup data files.

### Creating Directories

The `mkdir` command is used to make a directory. Enter `mkdir`
followed by a space, then the directory name you want to create.

~~~
$ mkdir backup
~~~
{: .bash}

### Moving / Renaming 

We can now move our backup file to this directory. We can
move files around using the command `mv`. 

~~~
$ mv SRR098026-copy.fastq backup
$ ls backup
~~~
{: .bash}
 
~~~
SRR098026-copy.fastq
~~~
{: .output}

The `mv` command is also how you rename files. Let's rename this file to make it clear that this is a backup.

~~~
$ cd backup
$ mv SRR098026-copy.fastq SRR098026-backup.fastq
$ ls
~~~
{: .bash}

~~~
SRR098026-backup.fastq
~~~
{: .output}

### File Permissions

We've now made a backup copy of our file, but just because we have two copies doesn't make us safe. We can still accidentally delete or 
overwrite both copies. To make sure we can't accidentally mess up this backup file, we're going to change the permissions on the file so
that we're only allowed to read (i.e. view) the file, not write to it (i.e. make new changes).

View the current permissions on a file using the `-l` (long) flag for the `ls` command. 

~~~
$ ls -l
~~~
{: .bash}

~~~
-rw-r--r-- 1 dcuser dcuser 43421 Jul 30 15:28 SRR098026-backup.fastq
~~~
{: .output}

The first part of the output for the `-l` flag gives you information about the file's current permissions. There are ten slots in the
permissions list. The first character in this list is related to file type, not permissions, so we'll ignore it for now. The next three
characters relate to the permissions that the file owner has, the next three relate to the permissions for group members, and the final
three characters specify what other users outside of your group can do with the file. We're going to concentrate on the three positions
that deal with your permissions (as the file owner). 

Here the three positions that relate to the file owner are `rw-`. The `r` means that you have permission to read the file, the `w` 
indicates that you have permission to write to (i.e. make changes to) the file, and the third position is a `-`, indicating that you 
don't have permission to carry out the ability encoded by that space (this is the space where `x` or executable ability is stored, we'll 
talk more about this in [a later lesson](http://www.datacarpentry.org/shell-genomics/05-writing-scripts/)).

Our goal for now is to change permissions on this file so that you no longer have `w` or write permissions. We can do this using the `chmod` (change mode) command and subtracting (`-`) the write permission `-w`. 

~~~
$ chmod -w SRR098026-backup.fastq
$ ls -l 
~~~
{: .bash}

~~~
-r--r--r-- 1 dcuser dcuser 43421 Jul 30 15:28 SRR098026-backup.fastq
~~~
{: .output}

### Removing

To prove to ourselves that you no longer have the ability to modify this file, try deleting it with the `rm` command.

~~~
$ rm backup/SRR098026-backup.fastq
~~~
{: .bash}

You'll be asked if you want to override your file permissions.

~~~
override r--r--r-- dcuser/dcuser backup/SRR098026-backup.fastq?
~~~
{: .output}

If you enter `n` (for no), the file will not be deleted. If you enter `y`, you will delete the file. This gives us an extra 
measure of security, as there is one more step between us and deleting our data files.

Important: The `rm` file permanently removes the file. Be careful with this command. It doesn't
just nicely put the files in the Trash. They're really gone.

By default, `rm`, will not delete directories. You can tell `rm` to
delete a directory using the `-r` (recursive) option. Let's delete the backup directory
we just made. 

Enter the following command:

~~~
rm -r backup
~~~
{: .bash}

This will delete not only the directory, but all files within the directory. If you have write-protected files in the directory, 
you will be asked whether you want to override your permission settings. 

> ## Exercise
>
> Do the following:
> 1. Make sure that you have deleted your backup directory and all files it contains.
> 1. Create a copy of each of your FASTQ files. (Note: You'll need to do this individually for each of the two FASTQ files. We haven't 
> learned yet how to do this
> with a wild-card.)
> 1. Use a wildcard to move all of your backup files to a new backup directory. 
> 1. Change the permissions on all of your backup files to be write-protected.
{: .challenge}
