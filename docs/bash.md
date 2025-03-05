% Bash Notes

[Home](../index.html)>[Hacking](index.html)

## Basics

- The &lt; command copies a file to standard input (which can be sent to a file that accepts it
    - &lt; and &gt; are for copying from standard io and files
    - Pipes are used to chain standard output of one command to standard input of another.
- You can use a backslash to continue a command onto two lines:

        echo hello \
        there


### Keyboard Combos

![](../assets/images/linux1.jpg)

- **CTRL-D** - used to end input
- **CTRL-(Backslash)** - Stop current command, if ctrl-c doesn't work
- **Ctrl-U** - **KILL LINE** - Cuts everything to the left 
- **Ctrl-W** - Cuts the word to the left 
- **Ctrl-Y** - Pastes what's in the buffer 
- **Ctrl-A** - Go to beginning of line 
- **Ctrl-E** - Go to end of line
- **ESC-?** - type in a letter, and this will show possible completions.
- **Alt-.** to insert the last used parameter from the previous line.
- **ESC-B** - Move one word backward
- **ESC-F** - Move one word forward
- **ESC-D** - Kill one word forward
- **CTRL-K** - Kill to end of line
- **CTRL-Y** -  Retrieve ("yank") last item killed
- **CTRL-D** - Delete one character forward
- **CTRL-L** -  Clears the screen, placing the current line at the top of the screen
- **CTRL-O** - Same as RETURN, then display next line in command history
    - Useful for repeating a sequence of commands you have already entered. 
    - Go back to the first command in the sequence and press CTRL-O instead of RETURN. 
    - This will execute the command and bring up the next command in the history file. 
    - Press CTRL-O again to enter this command and bring up the next one. 
- **CTRL-T** - Transpose two characters on either side of point and move point forward by one
- **CTRL-U** - Kills the line from the beginning to point
- **ESC-U** - Change word after point to all capital letters
- **ESC-L** - Change word after point to all lowercase letters
- **ESC-.** -  Insert last word in previous command line after point

 
- commands must always be separated by spaces: BAD: `[-f file]`  GOOD:`[ -f file ]`
- append a line to a file:
    - `echo 'PATH="$HOME/bin:$PATH"' >> "$HOME/.bashrc"`
- reload the shell:
    - `exec bash`
- `ls && cd` - Run CD only if ls succeeds
- `diff -u <(ls -c1 dir_1) <(ls -c1 dir_2)` - shows the diff between two lists of files
- `ls *.ps | xargs -P8 -I{} convert -density 150 {} {}.ppm` - P8 Parallel execution option for xargs.
- `cp ReallyLongFileNameYouDontWantToTypeTwice{,.orig}` - use the name expansion {} feature to save typing something twice

## Tricks

- Use hashtags to give "shortcut" names to long commands. Since hashtags are comments, they are ignored. But you can use Ctrl-R to search for them.


### Special character quick ref

- Single quotes - keep text inside from expansion or special characters.
    - `echo 'I am $LOGNAME'` yields `I am $LOGNAME`
- Double quotes - allow substitution
    - `echo "I am $LOGNAME"` yields `I am pnichols`
- semicolon - command separator - same effect as newline.
- | pipe - send the output of one command as the input to another 
- [[ expression ]] - Test expression. 
    - evaluates expression to "true" or "false"
- { commands; } : Command grouping. 
    - The commands inside the braces are treated as though they were only one command. 
    - It is convenient for places where Bash syntax requires only one command to be present, and you don't feel a function is warranted.
- backticks **or** $(command): Command substitution 
    - (The latter form is highly preferred.) 
    - Command substitution executes the inner command first, and then replaces the whole $(...) with that command's standard output.
- (command): Subshell Execution. 
    - This executes the command in a new bash shell, instead of in the current one. 
    - If the command causes side effects (like changing variables), those changes will have no effect on the current shell.
- ((expression)): Arithmetic Command.  
    - ((2 + 3)) 
- $((expression)): Arithmetic Substitution. 
    - Replaced with the result of its arithmetic evaluation. 
    - Example: `echo "The average is $(( (a+b)/2 ))"`.


### Types of commands

![](../assets/images/bash2.gif)

- **Alias** - String which is replaced with another string before being executed.
    - Only work in interactive shells, not scripts.
    - Replacement only works on first word.
- **Functions** - like mini shell commands.
    - Can be used in scripts, take arguments, set local variables.
- **Builtins** - built into bash - `cd`, `echo`, etc.
- **Keyword** - type of builtin with special parsing rules; a 'subunit of a parsing construct'.
- **Executables** - regular programs.

### Functions

- Not run in a subshell


### Scripts

- scripts should start with `#!/usr/bin/env bash`

### Variables

- You sometimes need to enclose the variable with braces, if it is followed by a character
- `echo ${NAME}alicious`
- `BOBalicious`
- When you use brackets, you can also use the substitution operators:
- `${varname:-word}`
    - If varname exists and isn't null, return its value; otherwise return word.
- `${varname:=word}`
    - If varname exists and isn't null, return its value; otherwise set it to word and then return its value. Positional and special parameters cannot be assigned this way.
- `${varname:?message}` 
    - If varname exists and isn't null, return its value; otherwise print varname: followed by message, and abort the current command or script (non-interactive shells only). Omitting message produces the default message parameter null or not set.
    - `filename=${1:?"filename missing."}`
- `${varname:+word}`
    - If varname exists and isn't null, return word; otherwise return null.
    - Testing for the existence of a variable.
    - `${count:+1}` returns 1 (which could mean "true") if count is defined.



### Positional Parameters

- $1, $2, $3 - first, second, third arguments to the script
- $0 - the name of the script itself
- `#` - variable storing the number of positional parameters
- $@ - expands to all positional parameters with double quotes


### login scripts

![](../assets/images/bash3.gif)

- `source` - allows you to reload bashrc or bash_profile
    - `.` is a synonym for source.
- alias name=command
    - **no spaces are allowed around the equals sign**
- Aliases can only be at the start of a command line
- Aliases can't be recursive (call other aliases)



### The '!' Command

        $ cd /home/user/foo
        cd: /home/user/foo: No such file or directory
        $ mkdir !*
        mkdir /home/user/foo

- `!*` - repeat all arguments
- `!:2` - repeat third argument (zero based)
- `!:2-5` - commands (3-6)  (zero based)
- `!:$` - repeat last argument
- `!!` - refers to last command
- `history` - display command history
- `!n` - execute command number N from history
- `^string1^string2` - Repeat the last command, replacing string1 with string2

