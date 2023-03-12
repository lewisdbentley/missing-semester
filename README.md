# The Missing Semester of My CS Education

## Course overview + the shell
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