---
layout: post
title:  "Terminals and Terminal Emulators"
category: extra
issueid: 5
date: 2024-4-4 12:00:00
--- 

Long ago, there was the [terminal](https://upload.wikimedia.org/wikipedia/commons/thumb/9/9f/DEC_VT100_terminal_transparent.png/865px-DEC_VT100_terminal_transparent.png). But everything changed, when the pixel-based displays attacked...

Terminals were the original method of interacting with programs: You type in input, programs respond with output. Over time, terminals implemented many additional features: limited foreground and background color support, modifying the cursor position, erasing content, and more. Where have these features gone when the pixel-based displays we know and love today came along?

Nowhere!

Today you have programs that can simulate (or emulate) these old terminals and draw the result in a window on your screen, hence terminal emulators. Along with that came surprisingly even more features. The XTerm terminal emulator is one of the base pieces of software in an X11 Linux system. Of course there came others, like Konsole for the KDE desktop, and some even implemented whole new protocols like Kitty, which allows displaying actual images in the terminal and downloading files from remote servers over just the terminal connection.

You can use these terminal features manually, but they are quite cumbersome to use, and there are libraries like ncurses to abstract them away. One of the more simple ones you can even include in your shell scripts easily:

````bash
echo -e '\e[31merror'
````

Prints "error" in red, useful for printing error messages that can be easily distinguishable from normal output. The `-e` parameter for echo is so it interprets the escape sequence `\e` as the ASCII ESC (escape) character, which all escape sequences have to start with. The ANSI control sequences then follow with an opening  square bracket, then parameters delimited by semicolons and then a character for the command name. In this case the command is "Select Graphic Rendition" and the only parameter is 31, which sets the foreground color to red. With 0 you can reset the color and other attributes if needed.

A list of the graphics commands can be found [here](https://en.wikipedia.org/wiki/ANSI_escape_code#SGR_(Select_Graphic_Rendition)_parameters).

With `\e[2K` you can clear the current line and reset the cursor to the beginning with `\e[G`, to display progress indicators like the curl progress bar for example.

`\e[3J` is an addition by XTerm, which deletes the entire screen and the scrollback buffer.
