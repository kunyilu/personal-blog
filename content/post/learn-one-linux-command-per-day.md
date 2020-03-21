---
title: "Learn One Linux Command per Day"
author: "Kunyi Lu"
date: 2020-03-14T23:51:15-07:00
draft: false
---

### vim
link: https://www.keycdn.com/blog/vim-commands

There are two modes in vim command: command mode and insert mode. We will go through some frequent usages. 

#### Basic commands
1. `:help [keyword]`: this command will help you find out the helpful documentation of the [keyword] you type. 
2. `:e [filename]`: this command will open a file. You need to provide the directory path of the file in the [filename].
3. `:w`: this command will save the current file.
4. `:wq`: this command will save the current file and quit vim.
5. `:q!`: this command will quit the vim without saving the file. 

#### Movement
1. `h`: move the cursor one charactor to the left. 
2. `l`: move the cursor one charactor to the right.
3. `j`: move the cursor down one line.
4. `k`: move the cursor up one line.
5. `b`: move the cursor one word to the left.
6. `w`: move the cursor one word to the right.
7. `0`: move the cursor to the beginning of the line.
8. `$`: move the cursor to the end of the line.
9. `gg`: move the cursor to the beginning of the file.
10. `G`: move the cursor the end of the file.
11. `:#`: # is the number of a line. Move the cursor to that line. 

You can add a count in front of one command to make Vim complete a command multiple times.

#### Editing
1. `v`: highlight one charactor per time
2. `V`: highlight one line per time
3. `y`: yank highlighted contents
4. `p`: paste whatever has been copied after the current line
5. `P`: paste whatever has been copied on the current line
5. `d`: delete highlighted contents
6. `dd`: delete current line
7. `u`: undo the last operation
8. `Ctrl+r`: Redo the last undo
9. `.`: repeat the last action

#### Searching
1. `/[keyword]`: searches for text in the document from the beginning to the end of the file.
2. `?[keyword]`: searches for text in the document from the end to the beginnign of the file.
3. `n`: Find next appearance
4. `N`: Find previous appearance
5. `:%s/[pattern]/[replacement]/gc`: replaces all occurrences of a pattern and confirms each one. 

- - -

### grep
You can use `grep` to search some pattern in a file.

#### Basic syntax
`grep pattern filename`

#### Useful arguments
1. `-n`: show matching line numbers
2. `-c`: count the number of occurrence
3. `-r`: perform recursive search
   > `grep -r "string" /home/`: search "string" in all files in the home directory and all its subdirctories.

#### Together with other commands
1. find out process pid
   > `ps -ef | grep someprocess`

- - -

### touch
You can use `touch` to create a new file, like `touch filename`

- - -

### sed
You can use `sed` to manipulte and edit text files without even opening them.

#### Basic syntax
`sed [operation] filename`

#### Useful usages
1. substitute or replace a string
   > `sed 's/pattern/replacement' filename`
   `s` in the command is the replacement indicator and `/` in the command is the delimiter

   By default, `sed` will only replace the first occurrence in a line.

2. substitute or replace a string(all occurrences)
   > `sed 's/pattern/replacement/g' filename`

3. substitute or replace a string in a specific range of lines
   > `sed 'line1,line2 s/pattern/replacement/g' filename` 
   The range is from line1 to line2.

- - -

### cat
`cat` is short for `concatenate`. You can use this command to show contents of a file or concatenate files together.

#### Basic syntax
`cat filename` <br/>
`cat > filename` <br/>

#### Useful usages
1. display contents of a file
   > `cat filename`

2. concatenate files together to a new file
   > `cat filename1 filename2 > newfile`

3. copy one file
   > `cat filename1 > filename2`
   `>` is printout redirection. It will redirect std out to file. `<` is printin redirection.

4. append one file to another
   > `cat filename1 >> filename2`

- - -

### ps
`ps` will report a snapshot of the current processes.

#### Basic syntax
`ps [options]`

#### Useful arguments
1. `ps -ef`: print out all processes. Use System V style.
2. `ps aux`: same as `ps -ef`. Use BSD sytle.

- - -

### top

- - -

### kill

- - -

### df

- - -

### chmod
`chmod` sets the permissions of files or directories. Permissions defines the permissions for owner of the file(user), members of the group who owns the file(group), and anyone else(others).

For example, if we want to set the permission like below:
1. User can read, write and execute
2. Group can read, write and execute
3. Others can read and execute

You can use command：`chmod u=rwx,g=rwx,o=rx filename` </br>
Or you can use equivalent command: `chmod 775 filename` where 4 stands for `read`, 2 stands for `write`, 1 stands for `execute`.

- - -

### ping
`ping` can test the connectivity to a specific destination computer by sending one or more ICMP Echo Request packages.

#### Basic syntax
`ping [options] URL`

- - -

### wget

`wget` lets you download files form the web. It supports background running.

#### Basic syntax
`wget [options] URL`

- - -

### curl
`curl` can transfer data from or to a server without user interaction.

#### Basic syntax
`curl [options] URL`

#### Useful usages
1. retrieve the homepage
   > `curl example.com`

2. save the output to a file
    > `curl -O url_address`
    > `curl -o filename url_address`
    `-O` will resue its original filename.

3. get the headers of a url
   > `curl -I  url_address`

- - -

### xargs
`xargs` allows you to build and execute commands from standard input. 

#### Useful usages
`echo "filename" | xargs touch` is equivalent to `touch filename`

- - -

### ssh

- - -

### find

- - -

### mount

- - -

### du


