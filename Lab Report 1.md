# How to log into a course-specific account on ieng6

## Step 1 - Logging into your course-specific CSE 15L account

Go to https://sdacs.ucsd.edu/~icc/index.php.
Enter your username and password into the given fields to access the list of your accounts. Under the subheading `Additional Accounts`, there should be a button labelled with an account name that begins with 'cse15lwi23'; click on this. 

If the page notifies you that a password has not been set, go ahead and set a password for the account. This might take a couple minutes to go into effect.

## Step 2 - Downloading Visual Studio Code and remotely connecting to the server

Visit https://code.visualstudio.com/ and follow the instructions on screen to download VSCode for your operating system. 

![Image](vscode.jpg)

Click on the `Terminal` tab and select the `New Terminal` option; a terminal should open at the bottom of your screen. 

Inside, type `ssh cs15lwi23zz@ieng6.ucsd.edu`, replacing the part before the '@' with your own username from Step 1, then press enter. Enter your password as prompted.

If this is your first time remotely connecting to the server, you will be met with a message that looks like this:

![Image](step%203_1.jpg)

If done correctly, you should be met with a screen that looks similar to this:

![Image](step%202.jpg)

You have connected to the UCSD remote server!

## Step 3 - Running commands

It might be good to practice running some commands to get used to using the server easily. Here is a list of commands and their functions:

`cd` - takes you to the home directory

`cd` <directory> - takes you to the specified directory

`cp` <file directory> - copies the given file

`cat` <file directory> - displays the contents of the given file
  
`ls` or `ls -t` - displays all files and directories in the current directory

`ls -a` - shows all files in the current directory, including files that begin with a ‘.’
 
`ls -l` - shows all files in the current directory with more information regarding file privacy and creation date

`ls -lat` - combines all aspects of the three commands above this one
  
Here is an example of the `ls -a` command:
 
![Image](ls%20and%20a.jpg)

Here is an example of the `cat` command displaying a file's contents:
  
![Image](cat_command.jpg)
  
Here is an example of the `cd` command going to a different directory:
  
![Image](cd_command.jpg)
