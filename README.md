# The Missing Semester of My CS Education

## 1. Course overview + the shell
1. Download Windows Subsytem for Linux. Try the command ```echo $SHELL.```
2. Create a new directory called missing under /tmp. Type ```mkdir /tmp missing```
3. Look up the touch program. ```man touch```
4. Use touch to create a new file called semester in missing. ```touch semester```
5. Write the following into that file, one line at a time: ```echo '#!/bin/sh && echo 'curl --head --silent https://missing.csail.mit.edu' > semester``` Note the use of single quotes: ! has a special meaning within double-quotes.
6. Try to execute the file. ```./semester``` output: Permission denied. 
7. Why does ```sh ./semester``` work, while ./semester didn’t?
8. Look up the chmod program ```man chmod```
9. Use chmod to make it possible to run the command. ```chmod +x semester``` How does your shell know that the file is supposed to be interpreted using sh? The loader executes the interpreter program specified after the shebang
10. write the “last modified” date output by semester into a file. ```./semester | grep last-modified | cut --delimiter=':' -f2- > /home/lewis/last-modified.txt``` Note: in this case take fields 2 until last
11. Read out your laptop battery’s power level or your desktop machine’s CPU temperature ```cat /sys/class/power_supply/battery/capacity``` Note: Could not find CPU temp

Notes:

## 2. Shell Tools and Scripting
1. An ```ls``` command that 
- Includes all files, including hidden files
- Sizes are listed in human readable format (e.g. 454M instead of 454279954)
- Files are ordered by recency
- Output is colorized
-- ```lsp () {
        ls -ahlt --color
}```

2. Whenever you execute marco the current working directory should be saved in some manner, then when you execute polo, no matter what directory you are in, polo should cd you back to the directory where you executed marco
-- ```marco () {
        marco_var=`pwd`
        echo "Saved marco"
}
polo () {
        cd "$marco_var"
        echo "Return polo"
}```

3. Run the following script until it fails and captures its standard output and error streams to files and prints everything at the end. Bonus points if you can also report how many runs it took.

```
#!/usr/bin/env bash

count=0

while true; do 
    count=$((count + 1)) 
    ./random.sh >log/stdout.txt 2> log/stderr.txt 
    if [[ $? -ne 0 ]]; then 
        echo -e "Script failed after $count runs.\nStandard output:\n$(cat log/stdout.txt)\nStandard error:\n$(cat log/stderr.txt)" 
        break 
    fi 
done
```

4. Recursively find all HTML files in the folder and makes a zip with them. Note that your command should work even if the files have spaces``` find . -name '*.html' -print0 | xargs -0 tar cvf html.tar.gz```

Notes
- ```test``` utility evaluates expressions. E.g. -f is true if a file exists.
- ```shellcheck``` will help you find errors in your sh/bash scripts
- ```tldr``` pages are a nifty complementary solution that focuses on giving example use cases
- ```fd```: the syntax to find a pattern PATTERN is fd PATTERN
- Stick with ripgrep ```rg```, given how fast and intuitive it is.
- make use of ```Ctrl+R``` to perform backwards search through your history. As you keep pressing it, you will cycle through the matches in your history.
- ```tree``` and ```broot``` exist to quickly get an overview of a directory structure
- Full fledged file managers like ```nnn``` use navigation arrows to quickly explore
- fasd adds a ```z``` command that you can use to quickly cd using a substring of a frecent directory. E.g. ```z lewis```
- ```history | rg {{something}}``` will print commands that contain the substring {{something}}.

## 2. Editors (Vim)
1. ```:help :w``` is different from ```:help w```: the former is for the command w (write) while the latter is for the w keystroke in normal mode
2. ```:qa``` closes all open windows
3. 
### Movement commands
3. ```w``` moves cursor forward one word. ```b``` moves back one word. ```e``` to end of word
4. ```0```beginning of line. ```$```end of line. ```^``` first non-empty character.
5. ```^D``` page down. ```^U``` page up. ```G``` moves all the way down. ```gg``` moves all the way up
6. ```fs``` find the next s. ```Fs``` find the previous s.
7. ```/range``` (+enter) cursor jumps to first instance of range

### Selection
Visual: v
Visual Line: V
Visual Block: Ctrl-v

### Edit commands
Vim’s editing commands are also called “verbs”, because verbs act on "nouns" (movements).
7. ```o``` insert a new line below, and insert. ```O``` insert new line above.
8. ```u``` undo (in normal mode). ```^R``` redo.
9. ```de``` delete until end of word. ```d$``` delete until end of line. ```ce``` delete until end of word, and insert. (c & d take motions as arguments)
10. ```cc``` delete line, and insert. ```dd``` delete line.
11. ```r!``` delete chosen character and replace with !. ```x``` delete chosen character.
12. ```yw``` copy the chosen word. ```p``` paste
13. ```.``` Repeat the last edit. (One of the most useful vim commands).

### Counts
13. ```8j``` move the cursor down eight lines (useful with relative line numbering).

### Modifiers
You can use modifiers to change the meaning of a noun
14. ```ci[``` change inside square brackets (enter insert mode after deleting contents within brackets)

## 1. Data Wranglng
1. ```less``` gives us a “pager” that allows us to scroll up and down through the long output.
2. ```sed``` basically gives short commands to modify a file, rather than manipulate its contents directly (although you can do that too).
3. ```sort -nk1,1``` means sort numerically and only in the first whitespace-separated column.
4. ```uniq -c``` collapse consecutive lines that are the same into a single line, prefixed with a count of the number of occurrences. (useful after applying sort).
5. ```paste -sd```  combine lines (-s) by a given single-character delimiter (-d; , in this case).