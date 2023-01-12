---
layout: post
title:  "Linux Shell Basics 3"
category: linux
issueid: 4
---


## Shell Scripting


Shell scripts are files with shell commands you can run, so you don't need to type all the commands every time you want to use it.  
Simple scripts just run the same commands each time you run them, but you can make scripts also accept parameters, read files, get input from the user and change the behavior according to that.  
Shells have some specifics for these things, and this post assumes you use bash.


### Simple Scripts


Let's start with a  script without any kind of input.  
Maybe you want an inspirational quote each morning when you start your terminal.  
A good start is to run and chain the commands in the terminal before writing them into a script, so you can run and modify them directly.  

I found a website that gives you a daily quote, let's download it with `curl`:

````bash
curl 'https://www.brainyquote.com/quote_of_the_day'
````

That spits out a lot of HTML.  
Parsing HTML with the shell is a bit tricky, but can work for simple patterns you want to find.  
I like to use GNU grep's perl regular expression support for that, because GNU grep is installed practically everywhere.  
You can use

````bash
grep -P -o -z -e '(?s)qotd-wrapper-cntr.*?<a.*?>.*?>\n*\K(.*?)(?=<.*?<\/a>)'
````

to extract all the quotes from the website. If you want you can use a website like regex101 that explains the regular expression, but you don't need to do that.  
With `head -n 1` we can then extract the first quote.  
You can chain the commands with pipes to get the first quote directly from the website.  

If you put
````bash
curl 'https://www.brainyquote.com/quote_of_the_day' | grep -P -o -z -e '(?s)qotd-wrapper-cntr.*?<a.*?>.*?>\n*\K(.*?)(?=<.*?<\/a>)' | head -n 1
````

in your ~/.bashrc, you get a quote every time you open a terminal.


### Changing Behavior

Getting a quote every time you open the terminal may be a bit much, so let's see how we can limit it to one per day.  
`date +%D` gets you the date. We just need a file  to store the last quote date, compare it with the current one and only output the quote when the dates differ. We also need to write the current date back into the file.  

To do that, we need to know about command substitution and if:

#### Command Substitution

With command substitution, you can use the output of a command to construct a command.  
It's especially useful to assign the output of a command to a variable.  
It works like `$(command)`, but you should use `"$(command)"` again to get correct handling of spaces.  
The command then gets executed before the rest of the line, end you can even nest them: `"$(cat $(cat filenames.txt))"`.


#### If

An if-block is used to execute commands only when a condition is true. The format is:


````bash
if program; then
    command_when_true
else
    command_when_true
fi
````

You can leave out the else-part if you don't need it.  
The exist status of the program after the if determines which path is taken.  
An exit code of 0 means the program completed successfully, and the true path is taken.  
Anything else is interpreted as false.  
  
To make these conditions easier, there's the shell builtin (or external program) `test` or `[ ]` that provides conditions you can use. You can use `man test` to read all of them. Some important ones:

````bash
[ -z "string" ]
````

Tests if the string is empty.


````bash
[ -n "string" ] or just [ "string" ]
````

Tests if the string is not empty.

````bash
[ "string1" = "string2" ]
````

Tests if string1 and string2 are equal.

````bash
[ "string1" != "string2" ]
````

Tests if string1 and string2 are not equal.


````bash
[ $? -ne 0 ]
````

`-ne` compares numbers for inequality, and `$?` is a special shell variable that is the return code of the last command.  
Use this to check if the last command failed.


#### If With Variables

You can use variables in the conditions the same like everywhere else.  
Now we can test if the date is the same:



````bash
lastquote="$(cat ~/lastquote.date 2>/dev/null)" # don't print the error message if the file doesn't exist
currentDate="$(date +%D)" # Get the date
if [ "$lastquote" != "$currentDate" ]; then # check if the last print was today
    # Print the quote
    curl 'https://www.brainyquote.com/quote_of_the_day' | grep -P -o -z -e '(?s)qotd-wrapper-cntr.*?<a.*?>.*?>\n*\K(.*?)(?=<.*?<\/a>)' | head -n 1
    echo "$currentDate" > ~/lastquote.date # write the date to the file
fi
````

### Loops

Sometimes you want to execute commands repeatedly without having to type them multiple times.  
There are 2 loops for this:

#### While

While is like if, but the commands get run again as long as the condition is true.  


````bash
# Loops endlessly and prints "While!"
while true; do
    echo "While!"
done
````

#### For

With for you can do something for every entry in a list. Lists in bash are just lists of words, where words are separated with spaces. You can combine this with another shell feature (globbing) to generate file lists in directories with files that match a pattern.


````bash
# Prints the names of all .txt files in the current directory
for file in *.txt; do # Using just * matches all names that don't start with a '.'
    echo "$file"
done
````



### Parameters

All parameters are available in the special variables `$x` where x is a number > 1.  
They are present in the order they are typed, which can make flags like seen in other programs difficult.  



````bash
# use GNU getopt to parse options
params="$(getopt -l 'long-varaiant,option-with-argument:' -o 'of:' -- "$@")"
if [ $? -ne 0 ]; then
    # getopt detected an error, so we exit. getopt should have already printed an error message
    exit 1
fi
# Set the parameters to the ones ordered and validated by getopt
eval set -- "$params"
# clean up the variable if you want
unset params

# Loop over the parameters
while true; do
    # case can compare a string with patterns or other string
    case "$1" in
    ('-o'|'--long-varaiant')
        # do something
        shift # shift the parameters to the left. $2 is now $1...
        ;;
    ('-f'|'--option-with-argument')
        # do something
        shift 2 # shift the parameters by 2, since $2 belongs to this option, too.
        ;;
    ('--')
        shift
        break # Break out of the loop, -- is the end of the options
    # * is the fallback pattern for everything
    (*)
        # Error, should not be able to happen
        exit 1
    esac # you have to end the cse block like this
done

# process the non-option parameters left in $1...


````


### Arithmetic

Sometimes you need to count things in a script. You can use arithmetic inside double brackets `(())`. If you need the result you can use `$(())`.


````bash
i=10
i=$((i + 1)) # $ is not needed inside (()) to use variables
((i += 1)) # you can also assign in (())
````

You can then use `-eq` and `-ne` in conditions to test numbers for equality and inequality, `-lt` and `-gt` for less than and greater than and `-le` and `-ge` for less or equal and greater or equal.


  
  
  

With this alone you should be able to make many functional scripts to automate things.  Here are some things you can look up if you need them:
- Heredocs: Useful for embedding large text in a script
- trap: If you don't want your script to be interruptible with ctrl + c
- Functions: if you want reusable code snippets you can give arguments to
- Arrays: if you want to process lists of things
- Process substitution: Pipe multiple programs into file arguments of another program


