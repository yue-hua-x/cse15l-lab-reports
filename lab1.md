for each example, I used `pwd` to display the current working directory. `pwd` stands for "print working directory" and does exactly what you'd think it does.
# the `cd` command
`cd` = change directory; this command changes your working directory to the one passed in as an argument, or, if there is no argument, changes your working directory to whichever directory is designated as your home directory.
## no arguments
```
[user@sahara ~/lecture1/messages]$ pwd
home/lecture1/messages
[user@sahara ~/lecture1/messages]$ cd
[user@sahara ~]$ 
```
with no arguments, the `cd` command automatically changes your working directory to your home directory. This output is not an error.
## directory path argument
```
[user@sahara ~/lecture1/messages]$ pwd
home/lecture1/messages
[user@sahara ~/lecture1/messages]$ cd ..
[user@sahara ~/lecture1]$ 
```
with a directory path as the argument, the `cd` command changes your working directory to the directory described in the argument. This output is not an error.
## file path argument
```
[user@sahara ~/lecture1/messages]$ pwd
home/lecture1/messages
[user@sahara ~/lecture1/messages]$ cd zh-cn.txt 
bash: cd: zh-cn.txt: Not a directory
[user@sahara ~/lecture1/messages]$ 
```
since a file is not a directory, `cd` doesn't do anything and instead prints a message on the terminal stating that the path points to something that is not a directory. This output is an error.

# the `ls` command
`ls` = list directory contents; this command lists the files and directories contained within the directory passed in as an argument, or, if there is no argument, the current working directory.
## no arguments
```
[user@sahara ~/lecture1/messages]$ pwd
home/lecture1/messages
[user@sahara ~/lecture1/messages]$ ls
cy.txt  en-us.txt  es-mx.txt  zh-cn.txt
[user@sahara ~/lecture1/messages]$
```
with no arguments, the `ls` command lists the files in the current working directory, which was all of the files in the messages folder. This output is not an error.
## directory path argument
```
[user@sahara ~/lecture1/messages]$ pwd
home/lecture1/messages
[user@sahara ~/lecture1/messages]$ ls ..
Hello.class  Hello.java  messages  README
[user@sahara ~/lecture1/messages]$
```
with a directory path as the argument, the `ls` command lists the files in the directory described in the argument. This output is not an error.
## file path argument
```
[user@sahara ~/lecture1/messages]$ pwd
home/lecture1/messages
[user@sahara ~/lecture1/messages]$ ls cy.txt
cy.txt
[user@sahara ~/lecture1/messages]$
```
with a file path as the argument, the `ls` command lists only the file. This output is not an error.

# the cat command
`cat` = concatenate files and print on standard output; this command outputs the content of the files passed to it as arguments; or, if there are no arguments, outputs the content of stdin
## no arguments
```
[user@sahara ~/lecture1/messages]$ pwd
home/lecture1/messages
[user@sahara ~/lecture1/messages]$ cat
yeehaw
yeehaw

```
with no arguments, `cat` starts "concatenating" the input from stdin; in this case, I entered "yeehaw" and `cat` repeated what I wrote. This output is not an error.
## directory path argument
```
[user@sahara ~/lecture1/messages]$ pwd
home/lecture1/messages
[user@sahara ~/lecture1/messages]$ cat ..
cat: ..: Is a directory
[user@sahara ~/lecture1/messages]$ 
```
with a directory as the argument, `cat` outputs an error message on the command line, stating that the path points to a directory. since `cat` is meant to output the contents of one or more files, and it doesn't make sense to output a directory, `cat` outputs an error.
## file path argument
```
[user@sahara ~/lecture1/messages]$ pwd
/home/lecture1/messages
[user@sahara ~/lecture1/messages]$ cat cy.txt 
Helo Byd
[user@sahara ~/lecture1/messages]$
```
with a file as the argument, `cat` outputs the contents of the file. This output is not an error.
