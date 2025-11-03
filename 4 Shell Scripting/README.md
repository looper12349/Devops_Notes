🐚 1. What is Shell Scripting?

Shell scripting means writing a series of commands for the Unix/Linux shell (command-line interpreter) to execute automatically.

A shell is a program that takes your commands (like ls, cd, mkdir) and tells the operating system what to do.

A shell script is simply a text file containing those commands, often with logic (loops, variables, conditionals, etc.)

Example: hello.sh

#!/bin/bash
echo "Hello, World!"


Then run:

chmod +x hello.sh     # make it executable
./hello.sh            # run it

🧠 2. What is Bash Shell?

Bash = Bourne Again Shell — the most common Linux shell.
It’s an improved version of the original sh (Bourne shell).

Other shells include:

sh – original Bourne shell

csh – C shell

ksh – Korn shell

zsh – Z shell (popular for customization)

Bash supports:

Variables

Control structures (if, for, while, etc.)

Functions

Command history and completion

⚙️ 3. What is .bashrc?

~/.bashrc is a configuration file that runs every time you open a new terminal session (non-login shell).

It contains user-specific shell preferences like:

Aliases

Environment variables

Custom functions

PATH updates

Example:

# ~/.bashrc
alias ll='ls -la'
export PATH=$PATH:/usr/local/bin


After editing, reload it with:

source ~/.bashrc

👥 4. Users and Groups in Linux

Linux is a multi-user system — every user has:

A username

A UID (User ID)

A group (GID)

Key concepts:

Each file or process belongs to an owner (user) and a group.

Groups allow sharing permissions among multiple users.

Commands:

whoami        # shows current user
id            # shows UID, GID, and groups
adduser bob   # create a new user (sudo needed)
groupadd devs # create a group
usermod -aG devs bob  # add user bob to devs group

🔐 5. Permissions in Linux

Each file/directory has 3 types of permissions for 3 types of users:

Category	Meaning
u	user (owner)
g	group
o	others
Permission	Symbol	Meaning
Read	r	4
Write	w	2
Execute	x	1
Example
-rwxr-xr--


Breakdown:

user → rwx (read, write, execute)

group → r-x (read, execute)

others → r-- (read only)

🧾 6. chmod – Change Permissions

Use chmod to modify permissions.

Symbolic method:

chmod u+x script.sh     # add execute for user
chmod g-w file.txt      # remove write for group
chmod o=r file.txt      # set read only for others


Numeric method:
Each permission has a numeric value (r=4, w=2, x=1).

Example:

chmod 755 script.sh 


Means:

user: 7 (4+2+1 = rwx)

group: 5 (4+0+1 = r-x)

others: 5 (4+0+1 = r-x)