---
layout: post
title:  "Termux Beginner Guide"
category: termux
---

Termux is a terminal emulator that uses the standard bash shell.  
All bash and general terminal principles apply also to Termux.  
In the rest of this post only the differences to a normal Linux system are explained.  
For information about the Linux shell, check out [my post about it]({% post_url 2022-12-20-linux-shell %}).

## Android ≠ Linux

The main difference is that the environment Android apps are run in is quite different from a standard Linux environment.  
Here are the main differences:


### Filesystem Layout

A normal Linux system has directories under the filesystem root: `/bin`, `/etc`, `/lib`, `/tmp` and `/home` are the most important ones.  
Android apps get no access to these directories, so Termux replicates this layout under it's own files, `/data/data/com.termux/files/usr`.  
The home directory is placed at `/data/data/com.termux/files/home` instead.  
Regular Linux programs expect the folders to be there under `/`, and don't work under Termux without configuring/patching them to use the new path.


### Users

A Linux system has a user for each, well, user.  
Android has a separate user for each app, and each "user" can only access that app's files.  
Typically Android apps also don't have access to the root user, which can be enabled by rooting your device.


### Instruction Set

Most Android devices are phones, which have processors designed by ARM with the arm64 instruction set.  
Desktop computers have mostly x86(_64) processors by Intel or AMD.  
That means even if the program you want to run doesn't need the standard linux file layout, your processor won't even know how to execute it.


### Limited System Information

For privacy reasons, apps cannot see other apps that are running on the system.  
That also translates to programs like `ps` in Termux, which can only list the programs running in Termux.  
Other parts of `/proc` are also blocked from apps for this reason.


### Limited System Calls[^1]

Newer Android versions also block some system calls for security, you might see `bad syscall` errors in Termux.


### Display

The display on a Linux system is managed by the X server or the Wayland compositor.  
Android has its own system for managing the display called SurfaceFlinger.  
This is incompatible with Linux programs, which is why you have to use an X server and view it with a VNC app, use Termux:X11 or other solutions to use graphical programs in Termux.





[^1]: If you don't know any terms, check out [the glossary](/linux-glossary.html)

