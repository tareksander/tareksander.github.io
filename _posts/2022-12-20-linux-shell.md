---
layout: post
title:  "Linux Shell Basics"
category: linux
issueid: 1
date: 2022-12-20 10:00:00
---

To interact with a terminal[^1], you need shell to accept your commands and run them.  
The most popular standard shell on Linux is Bash.  
Also commonly installed is a minimal set of programs:
- the GNU coreutils
    - `ls`
    - `echo`
    - `cat`
    - `cp`
    - `mv`
    - `mkdir`
    - `rmdir`
    - `rm`
    - `touch`{% comment %}
    - `env`
    - `head`
    - `printf`
    - `wc`
    - `uniq`
    - `timeout`{% endcomment %}
    - and many more...
- `find` to search in files and folders
- GNU `grep` to search for text with regular expressions
- `curl` or `wget`
- a command line editor
- a package manager

### Navigating Directories

For every line you can write commands in the shell (that is, not for every line that another program outputs), a prompt with some information will be displayed.  
Shells typically display the user you are logged in as and the path to the directory you are currently in.  
When you see `~` in the current path it means the user's home directory.  

To change the directory you can use the `cd` "command" (quotes, because it's not an actual program you can run, but rather provided by the shell itself).  
In contrast to windows, subdirectories and files are separated with `/` from the parent directory, not `\`.  
To change the directory, use `cd PATH`:
````bash
cd ~
cd linux
cd ..
cd linux/blog
cd ../..
````

A new line in these code snippets means you have  to press enter. Pressing enter lets the shell know you finished your command and the shell can now run it.  

This snippet first navigates to your home directory, then to the directory named "linux", then back out again.
`..` is a special directory that means "the parent directory", so you can easily navigate to parent directories.  

Then it navigates to the `blog` subdirectories of the `linux` directory. That demonstrates that you have to use `/`to separate a folder name from the sub-folder name.  

The last command navigates back to your home directory. It navigates to the parent directory of the parent directory, so 2 levels up, which is the home directory in this case.  

When your names include spaces, you have to enclose them with `""`: `cd "my path with spaces"`.  
The shell would interpret each word separated by the spaces as its own path otherwise.  
If your path includes \" or \$, you have to use `''` instead.

Shells also support something called tab completion, so you don't have to type as much.
When you enter a command or path and hit enter, the shell looks for command names or files/directories that match what you typed. If there is only 1 result to this search, the shell will complete the command or path for you.


## Creating and Deleting Directories

The above snippet likely doesn't work for you, as you may not have the directory `~/linux/blog`.  
To create a directory, use `mkdir PATH`, or `mkdir -p PATH` if your path contains a parent directory that doesn't exist.  
To delete an empty directory, use `rmdir PATH`.


## Creating and Deleting Files

To create an empty file, use `touch PATH`.  
To delete a file, use `rm PATH`.  
If you want to delete a folder with contents, use `rm -r PATH` (make sure you want to delete everything in it though).


## Options

The `-r` in `rm -r PATH` is an option. Options are optional information you can pass to programs. There are 2 types of options:
- Flags: These can be enabled with `-` and the option letter or `--LONGER_OPTION_NAME` for options with more than one letter
- Values: Like flags, but the next thing you specify is the value: `curl -o google.html https://www.google.com` would download https://www.google.com to  the file `google.html`.

## Copying and Moving

To copy use `cp SOURCE DESTINATION`.
To move, use `mv` instead.


## Listing Directory Contents

But how do you know which files and folders you can copy and move?  
You can use `ls PATH` to list them.  
For the current directory `ls` is enough.


## File Content

You can print files to the terminal with `cat FILE`, or edit them with an editor of your choice.  
Personally I use `nano` because displays the keys you need and I'm too lazy to learn a full terminal text editor.


## Downloading

To download files, you can use `curl` or `wget` with the URL you want to download:  
`curl -o google.html https://www.google.com`


## Advanced Usage

If you want do do something in the general theme of one of the applications I showed, chances are it can actually do it.  
Most applications support the flags `-h` or `--help` which shows more information about the application.  
For more in-depth information there is the separate command `man`, which you may have to install yourself.
Use it with `man COMMAND_NAME`.
  
  

That was an introduction into some basic commands. The next post is about useful shell features.

[Here is my next post in this series]({% post_url 2022-12-20-linux-shell-2 %}).


[^1]: If you don't know any terms, check out [the glossary](/linux-glossary.html)

