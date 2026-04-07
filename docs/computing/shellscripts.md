# Shell scripting notes

Notes on using the Unix shell (bash, zsh, et. al) to manipulate files,
text, etc.

 **See also:** [General programming page](programming.md), [the Awk page](awk.md), [Vim notes](vim_notes.md), [Text files](textfiles.md).

## Basic stuff

Assign variables like this:

    VAR="foo"  # don't use any spaces around the = sign 

Expand variables with $:

    echo $VAR # prints "foo" to stdout 

If the location of the variable is unclear in the input string use curly
braces:

~~~{.bash}
echo $VARbar # prints nothing
echo ${VAR}bar # prints "foobar"
~~~

Single quoted strings are interpreted literally:

~~~{.bash}
echo '$VAR' # prints "$VAR" to stdout
~~~

Double quoted strings are interpreted literally except for $, \`
(backquote), and \\ (escape).

~~~{.bash}
echo "$VAR" # prints "foo" to stdout 
~~~

Double quotes also group space separated words or variable together and
can be used to append/prepend:

~~~{.bash}
VAR2="bar foobar"
VAR3="foo$VAR2"
echo $VAR3 # prints "foobar foobar"
~~~

Backquoted strings are executed by your shell and are then replaced by
the output of that execution, so:

    echo The current date and time is: `date`

Outputs:

    The current date and time is: Thu Feb 24 23:19:17 MST 2011

The backslash escapes the metacharacter ($,\`,',") just after it so it
is interpreted as a string"

~~~{.bash}
echo $VAR # prints "$VAR" to stdout 
~~~

## Redirection and piping

The `>` operator redirects the standard output (or stderr sometimes)
of a command to a file.

    ls -l > ls-l.txt # Writes the output of the ls command to ls-l.txt

`>>` will append to a file, rather than clobber the existing file
with a new one. You can also set a "noclobber" option as the default in
bash.

Redirecting in the opposite directions, `<`, can be used to feed the
contents of a file into a command.

~~~{.bash}
cat < textfile1 > textfile2 # Same as "cp textfile1 textfile2"
~~~

The `|` operator "pipes" the standard output of a command to the
standard input of another command or process.

~~~{.bash}
ps axl | grep zombie # select and show zombie processes from the full list of processes
~~~


## Command line scripts

Simple shell scripts can be run from the command line, separating
commands with a semicolon.

 **Nice utilities to put in shell commands and scripts:**

* The usual file commands `cp`, `mv`, `rm`, etc.
* `grep`: prints text (from a file or standard input) matching a sequence of characters.
* `cut`: cuts columns out of delimited files
* `tr`: (translate) deletes (-d) or replaces characters in a file
* `tail`: prints the last //n// lines in a given file (good for removing headers).
* `sed` commands (see [this tutorial](http://www.grymoire.com/Unix/Sed.html))
* [awk oneliners](awk.md)
* `find`: ????

For example in a particular directory, a list of files with the .csv
extension can be generated with a simple loop:

~~~{.bash}
greg@gm-thinkpad:~/Downloads/station_inventory$ for i in *.csv; do echo "$i"; done
# Outputs:
AZ_soilstations.csv
CO_soilstations.csv
ID_soilstations.csv
MT_soilstations.csv
NM_soilstations.csv
NV_soilstations.csv
UT_soilstations.csv
WY_soilstations.csv
~~~

Using the same loop format, copies and substitutions to the filename can
be made:

~~~{.bash}
greg@gm-thinkpad:~/Downloads/station_inventory$ for i in *.csv; do cp "$i" "${i/.csv}2.csv"; done
# Outputs
greg@gm-thinkpad:~/Downloads/station_inventory$ ls
AZ_soilstations2.csv  MT_soilstations2.csv  UT_soilstations2.csv
AZ_soilstations.csv   MT_soilstations.csv   UT_soilstations.csv
CO_soilstations2.csv  NM_soilstations2.csv  WY_soilstations2.csv
CO_soilstations.csv   NM_soilstations.csv   WY_soilstations.csv
ID_soilstations2.csv  NV_soilstations2.csv
ID_soilstations.csv   NV_soilstations.csv
~~~

A similar effect to the substitution could be done with basename.

**Sed, Awk, and tr** all replace characters or patterns in textfiles. To remove all spaces from a directory of textfiles:

~~~{.bash}
for i in  .csv; do tr -d ' ' < "$i" > "${i/.csv}2.csv"; done
~~~

To replace each line's first occurrence of a given phrase with another pattern, either of these will work:

~~~{.bash}
for i in  *.csv; do sed 's/station id/station_id/' < "$i" >  "${i/.csv}2.csv"; done
~~~

OR  

~~~{.bash}
for i in  .csv; do awk -F "," '{ sub(/station id/, "station_id"); print }' "$i" > "${i/.csv}2.csv"; done
~~~

To replace all occurrences of the pattern use global replacement - add a `g` after the sed statement, or use `gsub` instead of `sub` in awk.

This command will take each .csv file, cut out columns 13-15, and
redirect this output to a new file.

~~~{.bash}
for FILE in  .csv; do cut -d"," -f-12,16- "$FILE" > "${FILE/.csv}2.csv"; done 
~~~

## Executing saved scripts

- Start scripts with the path to bash (#!/bin/bash)
- Make sure they are executable
- From the current working directory they can be run with "bash scriptname" OR "./scriptname
