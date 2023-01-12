# demo01 - Linux, Bash and Git Basics
If you do not have a Unix machine or WSL installed, you can probably follow along almost everything with the online emulator [copy.sh](https://copy.sh/v86/). Please select the Arch Linux distribution.

# Why DevOps in an OpenSource IC Class?
DevOps is a portmanteau of Development Operations: a branch of software developement that focuses on coding and infrastructure for the developement and operation of more software.
Since we are at the bleeding edge of IC developement, we are apart of building the tools and as such, we need to know a little about how the tools are managed. 

For our purposes we only need to be concered with two areas of DevOps: Version Control (git) and Virtualization (Docker).

# UNIX/Linux/BSD/OSX

[Family Tree](https://en.wikipedia.org/wiki/Berkeley_Software_Distribution#/media/File:Unix_history-simple.svg)


# Bash
[Bash Manual](https://www.gnu.org/software/bash/manual/bash.html)

Bash (Bourne Again SHell) is a shell derived from Sh, Ksh and Csh shells. First released in [1989](https://en.wikipedia.org/wiki/Bash_(Unix_shell)) by Brian Fox.
The basic idea of a shell is to expand and translate human readable commands into machine code for the compter to execute. Bash is also a scripting language that is turing complete. 

OSX is zsh and Ubuntu is bash now. No difference for our purposes.

### Navigation, File Maniplation and Directory Manipulation
```
echo 

pwd
# ~ is home and / is root

ls

clear

mkdir

cd

touch

mv

echo 'print("hello world")' >> hello.py
# Redirects

cat

# CTRL-C

```


### Useful Utilities
```
man

ps 

top

```

### Text Editing
```
nano

vi

vim

nvim
```

### Searching
``` 
grep

```

### Bash Shell Tools
Pipes: `|`

Standard Out Redirect with file overwrite: `>` 

Standard Out Redirect with file append: `>>` 

Command Substitution: `$(...)` substitues the output of the command `...` for `$(...)`.
Example: `echo $(pwd)`

Arithmetic Substitution: `$((...))` performes the arithmetic operation of `...` and substitues the answer.


### Bash Scripts
It's complicated. We'll only focus on the basics. 

[Shebang #!](https://en.wikipedia.org/wiki/Shebang_(Unix)) Is the first line of a shell script. It tells the executing shell, what type of shell to execute the script with. 
```
#!/bin/bash

#!/bin/zsh

# You can also tell the shell to use the python interpreter to execute the script
#!/bin/python

# Comments are dented with "#"

```


### Environment Variables

#### Basics
By convention all Bash environment variables are always in all caps. 

Environment variables are referenced by placing a `$` before the name. 

```
export

unset

env

```

#### `PATH`
Colons

### .bashrc/.zshrc
.bash_profile/.zsh_profile


### Permissions and Ownership
```
chmod

chown

```

# Git
### History and Structure
Git was created by Linus Torvalds in [2005](https://en.wikipedia.org/wiki/Git) (the same guy who created Linux).
We have created a [git cheatsheet](https://github.com/UAH-IC-Design-Team/documentation/wiki/Git-Cheat-Sheet) with all of the most common git workflows and commands needed for this class.

Branches

Repos


### Local 
```
git help

git init

git status

git add

git commit

git stash

git branch

git checkout

git merge
# conflicts

# .gitignore

git reset

git revert

git restore
# Restore is a newer git feature that has some nice parts, but may change in the future.
```

### Lexicon
- **stash:** Temporary dumping ground for temporary changes
- **working tree/workspace/working copy:** current state with all un-indexed and un-commited changes. This is where all of the development happens. The working tree comes befor ethe index.
- **index/staging area:** added/modified/deleted files that are staged to be commited. This is inbetween the working tree and the local repository.
- **local repository:** is the full set of commits and branches of the project. All data is hashed and stored in the `.git` directory.
- **HEAD:** the current branch and commit of the working tree. Can be found in the `.git/HEAD` file.
- **commit:** is a checkpoint, in the the development of a project that can be returned to.
- **branch:** a collection of commits that generally are grouped to form one cohesive part of a project.

[Git reset, restore and revert](https://git-scm.com/docs/git#_reset_restore_and_revert)



