---
layout: post
title:  "Linux Shell Basics"
category: linux
---

To interact with a terminal[^1], you need shell to accept your commands and run them.  
The most popular standard shell on Linux is Bash.  
Also commonly installed is a minimal set of programs:
- the GNU coreutils
    - `ls`
    - `echo`
    - `cat`
    - `chmod`
    - `chown`
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

Then it navigates to the `blog` subdiredory of the `linux` directory. That demonstates that you have to use `/`to separate a folder name from the subfolder name.  

The last command navigates back to your home directory. It navigates to the parent directory of the parent directory, so 2 levels up, which is the home directory in this case.  

When your names include spaces, you have to enclose them with \"\": `cd "my path with spaces"`.  
The shell would interpret each word separated by the spaces as its own path otherwise.  

Shells also support something called tab completion, so you don't have to type as much.
When you enter a command or path and hit enter, the shell looks for command names or files/directories that match what you typed. If there is only 1 result to this search, the shell will complete the command or path for you.


## Creating and Deleting Directories

The above snippet likely doesn't work for you, as you may not have the directory `~/linux/blog`.




[^1]: If you don't know any terms, check out [the glossary]({% post_url 2022-12-19-linux-glossary %})

