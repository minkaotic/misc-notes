# Linux Command Line Wizz
[Package management](#package-management) | [Processes](#processes) | [Permissions](#permissions) | [Environment](#environment) | [Find & Grep](#find--grep) | [Pipes & Redirection](#pipes--redirection)

**Resources:**
- [Codecademy command line glossary](https://www.codecademy.com/articles/command-line-commands)
- [Command Line Challenge](https://cmdchallenge.com/)
- [Console Foundations on Treehouse](https://teamtreehouse.com/library/console-foundations)
- [`grep` by example](https://www.shortcutfoo.com/blog/grep-by-example)

## Package management
- `sudo apt-get update && apt-get upgrade` - update package index and then upgrade all currently installed software packages (add  package name to only upgrade specific package)
- `apt-cache search [package]` - finds matching software package names / descriptions
- `apt-cache show [package]` - show package information, incl. version number, installed size, origin, maintainer etc.
- `sudo apt-get autoremove` - removes dependencies of uninstalled packages
- `sudo apt-get autoclean` - free disk space by clearing out deprecated .deb files retrieved in the past (swap autoclean for 'clean' to get rid of all retrieved .deb files)
- `sudo apt-get install [package]=[version]` - install specific version
- `sudo apt-get remove --purge [package name]` - completely uninstall a package (drop '--purge' in order to leave configurations behind for reinstall)
- `apt-get install --only-upgrade [package name]` - upgrade to latest version

## Processes
- `ps` - show snapshot of running processes of your current console session
- `ps aux` - show snapshot of all running processes at the time
- `ps aux | grep "[search term]"` - filter by terms, such as process name
- `top` - auto-updating view of all processes; like task manager
- `jobs` - lists all processes in your current console session and their job ids
- `ctrl + z` while in a console program (nano etc.): pause the program
- `fg` - brings the most recently paused program to the **f**ore**g**round
- `fg [job id]` - resumes (foregrounds) the selected program
- `[program command] &` - starts and pauses a program, so it gets added to the list of paused jobs. For example: `vim hello.txt &`
- `kill [process id]` (equivalent: `kill -TERM [process id]`) - sends the terminate signal to the process
- `kill -KILL [process id]` - hard kill, aka `kill -9 [process id]`
- `kill -STOP [process id]` - sends the stop signal (to pause or background a process), even if it that process is not part of our current terminal session

## Permissions
- `chmod` - modify permissions, eg. `chmod 640 [file name]` or `chmod u+w [file name]` (with r/w/x => 4/2/1, one group each for user, group and other)
- `chown` - modify owner
- `adduser` - launches interactive process to create a new user

## Environment
- `VARIABLE=value` - set a local environment variable
- `export VARIABLE=value` - set an environment variable that will be visible to child processes
- `env` - view environment variables
- `$PATH` is an environment variable that shows the list of directories separated by `:` to search for when an executable is run
- `which` - print the location of a program, e.g. `which echo`
- To temporarily add a new directory to the (in this case start of the) PATH: `export PATH=/some/path/bin:$PATH` - to do this *permanently*, add this line to your `.bashrc` file which runs every time you open the console.
- `bash` - start a new session within your current session
- `echo` - display the arguments sent to echo, e.g. `echo $PS1`

## Find & Grep
#### `find` allows us to locate a file, based on its name, using patterns.
E.g.: `find . -name "search"` - look for files with the name search starting from your current location, or `find / -name "search"` to search the whole system.
- `*` works as a wildcard anywhere in the search term for `find`

#### `grep` lets us search within files (or other input) for a certain pattern.
E.g. `grep "pattern" filename` - find any lines that contain the pattern in the given file
- `-n` - print line number in result, e.g. `grep -n "pattern" filename`
- `-i` - perform a case insensitive search
- `-v` - only show lines _without_ the pattern

## Pipes & Redirection
- `somecommand < inputfile` - run somecommand with input from inputfile, instead of `stin` (i.e. the keyboard)
- `somecommand > outputfile` - run somecommand whilst redirecting `stout` to the outputfile instead of the terminal screen - **overwriting** any existing content in the outputfile
- `somecommand >> outputfile` - run somecommand whilst redirecting `stout` to **append after** any existing content in the outputfile
- `somecommand 2> outputfile` - redirect the `sterr` (standard **error**) output stream to an output file (use `2>>` to append); `stout` will still be directed to the screen :)
- `command1 | command2` - pipe the output of command1 to the input of command2 (i.e., take the `stout` of command1 and use it as the `stin` of command2)
- `somecommand 2> /dev/null` - discard `sterr` by redirecting it to the black hole of nothingness
