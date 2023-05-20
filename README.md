# The Missing Semester of My CS Education

## - Course overview + the shell
- Download Windows Subsytem for Linux. Try the command ```echo $SHELL.```
- Create a new directory called missing under /tmp. Type ```mkdir /tmp missing```
- Look up the touch program. ```man touch```
- Use touch to create a new file called semester in missing. ```touch semester```
- Write the following into that file, one line at a time: ```echo '#!/bin/sh && echo 'curl --head --silent https://missing.csail.mit.edu' > semester``` Note the use of single quotes: ! has a special meaning within double-quotes.
- Try to execute the file. ```./semester``` output: Permission denied. 
- Why does ```sh ./semester``` work, while ./semester didn’t?
- Look up the chmod program ```man chmod```
- Use chmod to make it possible to run the command. ```chmod +x semester``` How does your shell know that the file is supposed to be interpreted using sh? The loader executes the interpreter program specified after the shebang
-. write the “last modified” date output by semester into a file. ```./semester | grep last-modified | cut --delimiter=':' -f2- > /home/lewis/last-modified.txt``` Note: in this case take fields 2 until last
-. Read out your laptop battery’s power level or your desktop machine’s CPU temperature ```cat /sys/class/power_supply/battery/capacity``` Note: Could not find CPU temp

Notes:

## - Shell Tools and Scripting
- An ```ls``` command that - Includes all files, including hidden files - Sizes are listed in human readable format (e.g. -4M instead of 454279954) - Files are ordered by recency - Output is colorized -- ```lsp () { ls -ahlt --color }```

- Whenever you execute marco the current working directory should be saved in some manner, then when you execute polo, no matter what directory you are in, polo should cd you back to the directory where you executed marco
```marco () { marco_var=`pwd` echo "Saved marco" } polo () { cd "$marco_var" echo "Return polo" }```

- Run the following script until it fails and captures its standard output and error streams to files and prints everything at the end. Bonus points if you can also report how many runs it took.

```
#!/usr/bin/env bash

count=0

while true; do 
    count=$((count + -) 
    ./random.sh >log/stdout.txt - log/stderr.txt 
    if [[ $? -ne 0 ]]; then 
        echo -e "Script failed after $count runs.\nStandard output:\n$(cat log/stdout.txt)\nStandard error:\n$(cat log/stderr.txt)" 
        break 
    fi 
done
```

- Recursively find all HTML files in the folder and makes a zip with them. Note that your command should work even if the files have spaces``` find . -name '*.html' -print0 | xargs -0 tar cvf html.tar.gz```

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

## - Editors (Vim)
- ```:help :w``` is different from ```:help w```: the former is for the command w (write) while the latter is for the w keystroke in normal mode
- ```:qa``` closes all open windows
- 
### Movement commands
- ```w``` moves cursor forward one word. ```b``` moves back one word. ```e``` to end of word
- ```0```beginning of line. ```$```end of line. ```^``` first non-empty character.
- ```^D``` page down. ```^U``` page up. ```G``` moves all the way down. ```gg``` moves all the way up
- ```fs``` find the next s. ```Fs``` find the previous s.
- ```/range``` (+enter) cursor jumps to first instance of range

### Selection
Visual: v
Visual Line: V
Visual Block: Ctrl-v

### Edit commands
Vim’s editing commands are also called “verbs”, because verbs act on "nouns" (movements).
- ```o``` insert a new line below, and insert. ```O``` insert new line above.
- ```u``` undo (in normal mode). ```^R``` redo.
- ```de``` delete until end of word. ```d$``` delete until end of line. ```ce``` delete until end of word, and insert. (c & d take motions as arguments)
-. ```cc``` delete line, and insert. ```dd``` delete line.
-. ```r!``` delete chosen character and replace with !. ```x``` delete chosen character.
-. ```yw``` copy the chosen word. ```p``` paste
-. ```.``` Repeat the last edit. (One of the most useful vim commands).

### Counts
-. ```8j``` move the cursor down eight lines (useful with relative line numbering).

### Modifiers
You can use modifiers to change the meaning of a noun
-. ```ci[``` change inside square brackets (enter insert mode after deleting contents within brackets)

Notes

## Data Wrangling
- (1)
- Take this short interactive regex tutorial (done).
- (2)
- Find the num of words in /usr/share/dict/words with three a's which do not end in 's. ```cat /usr/share/dict/words | rg a.*a.*a.*[^\'s]$ | wc --words```
- What are the three most common last two letters of these words? 
```cat /usr/share/dict/words |``` prints the contents of the file words to stdout.
```tr "[:upper:]" "[:lower:]"``` converts upper case to lower cas
```rg ^.*a.*a.*a.* |```  find all the words which contain three a's
```rg -v "'s$" |``` find all the words which do not end in 's using reVerse
- How many of those two-letter combinations are there? 
```sed -E "s/.*([a-z]{2})$/\1\" | ``` replace all the lines with capture pattern of last two a-z characters
```sort | uniq -c | ``` sort and display number of occurences
```sort -n |``` sort by number of occurences (ascending)
```tail -n3``` only show the three most common two letter patterns
And for a challenge: which combinations do not occur?
- ``` ... | sort | uniq > last_letters.txt ``` to print the letter combinations which do occur to a file
- Write a small program to collect all the possible two letter combinations:
```
#!/usr/bin/env bash

letters=('a' 'b' 'c' 'd' 'e' 'f' 'g' 'h' 'i' 'j' 'k' 'l' 'm' 'n' 'o' 'p' 'q' 'r' 's' 't' 'u' 'v' 'w' 'x' 'y' 'z')

length=${#letters[@]}

for ((i = 0; i < length; i++)); do
    for ((j = 0; j < length; j++)); do
        # echo "Letter combo: $((length * i + j)): 
        echo "${letters[i]}${letters[j]}"
    done
done
```

- Output the two-letter combinations to a file ```source letters.sh all_letters.txt```
- Check the difference between both all_letters and last_letters ```diff --changed-group-format="%<" --unchanged-group-format="" all_letters.txt last_letters.txt
- (3)
It is tempting to edit files in-place via ```sed/REGEX/SUBSTITUTION/ input.txt > input.txt``` However, this fails because an empty input.txt will output to an empty input.txt. Furthermore, editing files in place can be dangerous, even without getting regexes involved. Nevertheless, the -i extension can be used if desired. ``` sed -i s/stars/étoile/g auden.txt```
Notes
- if you want some simple plotting, gnuplot is your friend
- ```less``` gives us a “pager” that allows us to scroll up and down through the long output.
- ```sed``` basically gives short commands to modify a file, rather than manipulate its contents directly (although you can do that to)
- ```sort -nk1,1``` means sort numerically and only in the first whitespace-separated column.
- ```uniq -c``` collapse consecutive lines that are the same into a single line, prefixed with a count of the number of occurrences. (useful after applying sort).
- ```paste -sd```  combine lines (-s) by a given single-character delimiter (-d; , in this case).

