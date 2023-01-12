---
layout: post
title:  "Linux Shell Basics 2"
category: linux
issueid: 3
date: 2022-12-20 12:00:00
---


Remember: If you don't know any terms, check out [the glossary](/linux-glossary.html).  
  

### Environment Variables

Environment variables are simple pairs of a name and a value that are passed to each command you run.  
You can use the command `env` to print all current ones out.  
You can define new ones with `export NAME=VALUE`.  
You can also define exported variables for one command only: `A=aaaa env` will also list the variable `A`.


### Shell Variables

Shell variables are like environment variables, but are not passed to commands you run.  
You can set them with `NAME=VALUE`.  
To use a shell or environment variable, use `$NAME`, e.g. `echo $HOME` to print the home directory of the user.  
`echo` is another "command" that's usually not a separate program, but build into the shell for things like printing variables.  
When you want to use 2 variables directly after each other, you have to also enclose them in {} so the shell knows it's not just one name: `echo ${HOME}${USER}`


### In-Depth Shell Syntax

Now that you know basic commands and how to read their help test, it is time to re-visit the command syntax.  
A shell command is build like this:  
`[ENVIRONMENT_VARIABLES] command [parameter] [parameter] ...`  
The parts in [] are optional, and ... means you can add as many parameters as you like (there is technically a limit, but you won't reach that when manually typing commands).  
The spaces are significant: Parameters, the command and the environment variable definitions are all separated by at least one space. When you want something to include spaces, surround it in `""`.
  
One handy thing is that variables are replaced before anything about the command gets run:

````bash
LS="ls -l"
$LS ..
````

This will run `ls -l ..`, because `$LS` gets evaluated first.  

But what if you want to print literally $HOME?  
You can use `''` which suppresses variable replacement: `echo '$HOME'`.  

When using variables where you don't know if the value contains spaces, always encase them in `""`, or you might get nasty surprises:

````bash
file="my super cool filename with spaces.txt"
cat $file
````
This will try to print the contents of the files `my`, `super`, `cool`, `filename`, `with`, `spaces.txt`, not  `my super cool filename with spaces.txt`.

Sometimes you want the parts of the variable value to be treated as separate though:

````bash
ls_options="-a -l"
ls "$ls_options"
````

This would make `ls` complain about an invalid option.


### Redirection


To understand redirection, we have to make a small detour over file descriptors:

#### File Descriptors

Whenever a program opens a file, it gets a positive number back that references this file in the operating system for the program. That number is a file descriptor.  
The first 3 file descriptors have a standard meaning:
- 0 is the standard input, from which programs can read.
- 1 is the standard output, to which programs can write.
- 2 is the standard error, to which programs can write error messages, so they don't get mixed up with the normal output.

Programs can close file descriptors to make the number available for a new file and duplicate a file descriptor to another number, so they reference the same actual file.  
The shell exposes this in the form of I/O redirection.


#### Shell Redirection

Let's say a program writes to the standard output, but you only want the error messages.  
You could redirect the standard output away if you want to keep it.  
Closing it would maybe make the program crash, as it expects to be able to write the standard output (it's **standard**, yknow?).  
For this there are 2 special files:
- `/dev/null`: Acts as an infinite sink for data. The operating system just discards any data written to this file.
- `/dev/zero`: An infinite source of 0 bytes. Writes are also discarded.

You can use `FD<NAME` to open the file descriptor FD for reading (piping data into FD) from the file NAME and `FD>NAME` to make writes go to the file NAME (piping from FD to the file NAME).  
When writing, the file gets overwritten. If you want to append the data instead, use `FD>>NAME`.  
If `FD` is not given, 0 is assumed for < and 1 for > and \>\>.  
  
You can also specify another file descriptor on the right side, but you have to use `&` so the shell knows it's not a filename: `FD1<&FD2`.

With that knowledge we can now pipe away the pesky output into oblivion!  
`ls >/dev/null` will print nothing, which is exactly what we were after in this case.

Another case is where you don't want the error information and just the normal output: `COMMAND 2>/dev/null` will discard the standard error for COMMAND.  


And lastly, you may not want any output from a program:
- `COMMAND >/dev/null 2>/dev/null`: Works, but is much to type.
- `COMMAND >/dev/null 2>&1`: First redirects standard output to `/dev/null` and then standard error to standard output, which we set to `/dev/null`.
- `COMMAND 2>&1 >/dev/null`: This outputs the standard error to the terminal, as we're first piping standard error top standard output and then standard output to `/dev/null`. The shell applies the redirection in the order you give them.



### Combining Programs With Pipes

Linux terminal programs tend to specialize in one thing or area, but complex tasks are still possible by combining them.  
The shell can transfer the standard output of one program into the standard input of the next program with `|`.  
  
Let's say you have a copy of Mobby-Dick in the file `mobby.txt` and you want to find all lines where "Mobby" is used and send them to a friend. Sadly, Emails don't support such large attachments.  
You could do

````bash
grep Mobby mobby.txt > found.txt
gzip found.txt
````

or combine it onto one line:

````bash
grep Mobby mobby.txt | gzip > found.gz
````

If your friend wants to send you back the lines that also contain "whale", they could do

````bash
gunzip -c found.gz | grep whale | gzip > found_whale.gz
````

and send you back the file without clobbering the directory with temporary uncompressed files.

  
  


These are the main handy features for interactive shell use. The next post is about creating shell scripts.


[Here is my next post in this series]({% post_url 2023-01-12-linux-shell-3 %}).

