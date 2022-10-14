# Getting comfortable with your UNIX terminal: the Bash shell

A shell is a program that interprets and executes commands. `Bash` is a popular shell program and scripting language, and is the default for most linux and Mac operating systems. Below, we'll cover some basic commands that you'll use on a day-to-day basis, and touch on how to customize your shell to add, e.g., color, custom commands (aliases), and to remotely connect to other machines (via ssh).

Most shell programs can be run interactively, or via scripts that contain sequential commands. Below, we'll mostly work interactively. Note that this is really just a cursory introduction to working with the `Bash` shell! I highly recommend reading through a tutorial, [like this one](http://cs.lmu.edu/~ray/notes/bash/) to see all of the cool things you can do with a shell.

Let's now all start up a terminal. On a Mac, you can use the pre-installed Terminal application or download and install a third-party terminal software like [iterm2](https://www.iterm2.com/). On Linux, open the default Terminal application. On Windows, we'll be working with the Ubuntu subsystem, so open up the terminal / Ubuntu program you installed.

You should now have a terminal open and see the blinking cursor eagerly awaiting input.


## Basic commands

When you first open a shell, you will be in some default path. Usually this is your home folder / directory. To check what path you are in, also known as the current or present working directory, use the `pwd` command. In your shell, type this command and hit enter - you should see something like the below (it may start with /home/... instead):

    $ pwd
    /Users/adrian

To find out what files and folders are in your current directory, use the `ls` command - you should see a bunch of names print out:

    $ ls
    Applications  
    Desktop      
    Documents    
    Downloads    
    Dropbox      
    ...etc...

We can also list the contents of any other directory on your machine by specifying a path after the command. For example, to see what is in your Documents folder (you might not have one), you can do:

    $ ls Documents
    master.tar.gz  photo.jpg      sample_cmd.png
    ...etc...

Let's say I have many image files in there, and I want to know what PNG images are in my documents path. I can list sub-selections of files using a "wildcard" operator, the * symbol. This symbol basically means "match anything in replace of the *". For example, I could find all .png files by doing:

    $ ls Documents/*.png
    Documents/sample_cmd.png

You can get a lot more information out of `ls` using various flags. One I use very frequently is the `-l` flag:

    $ ls -l Documents
    -rw-r--r--  1 adrian  staff  165359616 Aug 22  2016 master.tar.gz
    -rw-r--r--@ 1 adrian  staff     116337 Feb  6  2017 photo.jpg
    -rw-r--r--@ 1 adrian  staff     441487 Jun  3  2017 sample_cmd.pn

This shows some cryptic information, then the owner of the file, the filesize, the time it was last modified, and then the filenames.

Notice I had to type the name of the Documents folder each time because that is a subdirectory within the path that I am currently in (/Users/adrian). In any shell, you can also move your current working directory using the `cd` (change directory) command. For example, I can move into the Documents folder:

    $ cd Documents
    $ pwd
    /Users/adrian/Documents
    $ ls *.png
    Documents/sample_cmd.png

You don't always have to type the full name of a directory to go to it. Now that we're in the Documents folder, if I want to go back up to my home directory, I can type either:

    $ cd ..
    $ pwd
    /Users/adrian

The `..` is shorthand for "the directory above my current one". You can string these together to go up several directories. For example, we'll go back down in to my Documents folder, then go all the way up to the /Users/ folder:

    $ cd Documents
    $ cd ../../
    $ pwd
    /Users

Another good shorthand to know is the short name for "home directory": `~/`

    $ cd ~/
    $ pwd
    /Users/adrian

The most common commands I use in a shell are:

* `ls` - to list the contents of a directory
* `cd` - to change the current working directory
* `cp` - to copy a file or directory
* `mv` - to move a file or directory (_be careful with this one_)
* `rm` - to delete a file or directory (_be careful with this one_)
* `man` - to display the manual (help) for a command or program
* `wc` - word count or line count (with the `-l` flag)
* `less` - show part of a text file
* `cat` - dump the contents of a file
* `touch` - create an empty file with the specified name
* `ln -s` - create a "soft" link from one file to a new file in the current directory

A few more, ordered by application, are listed in this [cheat sheet](https://www.dropbox.com/s/k7athu9i8lmmeln/Linux%20Cheat%20Sheet%20David%20Relyea.pdf).

For the following part, we will use one more command, which allows us to create a directory 'mkdir'. From your home directory create a work directory and move to this directory:
	
	mkdir Directory_name
	cd Directory_name

## Text editors (for making quick changes)

Graphical text editors such as ([Atom](https://atom.io/)) have a bunch of nifty, customizable features. Many people instead swear by using shell-based text editors, like `emacs` or `vim`. I don't care what you use, but it's important that you learn the basics in at least one of the shell-based text editors, at least for making quick edits or edits on remote machines. Your options are (to name a few):

* `emacs`
* `vim`
* `nano`
* `sublime`

If you go with `emacs` (that's what I use occasionally), the most important things to know are how to save, and how to quit. To open `emacs`, type the command name with a filename. For example, to edit a file "test.txt" in the current directory:

    $ cd ~/
    $ emacs test.txt

Type some words in there. Then, to save, do:

    ctrl + x, s

In the above, hold ctrl and hit x, then hit s.

To quit `emacs`, the command sequence is:

    ctrl + x, c


Most of the above commands are not actually part of `bash`, but are programs that can be run from within a `bash` shell. So how does `bash` know about these scripts?

## Environment variables

All shells allow setting variables that store values. There are also a number of special variable names that control aspects of the shell. To set an environment variable, you just have to set some value to a name with the = operator:

    $ myvariable=14

We can dump the contents of this variable with the `echo` command:

    $ echo $myvariable
    14

Note that when I want to work with an env variable (after setting), the name is prefaced with a dollar sign!

So how does your `bash` shell know about all of those scripts (like `cat`) that aren't part of the `bash` shell? Well, there is a special environment variable called `$PATH`, and this tells your shell where to look for program names when you type something in the terminal prompt. Dump the contents of this variable:

    $ echo $PATH
    .../usr/local/bin:/usr/bin:/bin...

It should contain a bunch of things that contain `/bin` paths. List the contents of one of those, like `/bin`:

    $ ls /bin
    [          chmod      date       domainname expr       ksh        ln         mv         pwd        sh         sync       unlink
bash       cp         dd         echo       hostname   launchctl  ls         pax        rm         sleep      tcsh       wait4path
cat        csh        df         ed         kill       link       mkdir      ps         rmdir      stty       test       zsh

Aha! So all of those commands we were playing with above are just executable programs that like in a path that `Bash` knows about. You can add other paths to your `$PATH` variable to make `Bash` aware of other program locations. __But beware__: if you accidentally delete the contents of the variable, it can be a huge pain to fix!

## Startup and configuration files

Every time you open a `Bash` shell, `Bash` executes a few files to help set up the shell. One of those files, `~/.bash_profile` (i.e. the file `.bash_profile` in your home directory), is customizable, and so a place you can put things like shell configuration and to set custom environment variables. From this file, you can also execute other scripts or commands.

For example, in my profile file, I have the following lines (amongst others):

    export CLICOLOR=1
    export LSCOLORS=ExFxCxDxBxegedabagacad

    source ~/.prompt

The first two lines tell the shell to color the output of `ls` commands.

The last line runs the "`source`" command on the file `~/.prompt`. `source` will execute the contents of the specified file, so this just executes the file `~/.prompt`. The contents of this file [can be seen here](https://github.com/adrn/dotfiles/blob/master/bash/prompt): this changes the color and content of my terminal prompt to add some nifty features.


## SSH and remote login

We will now learn how to connect between your laptop and your department machine. That's what ssh does. With this you will be able to exchange files between your machine and the department machine. This is particularly useful if you need to do computation on datasets that are too large for your laptop to handle for instance. This way, your laptop can be used to develop code or use programs that are already installed on your machine. 

The detailed informations about ssh and remote login are described in the following:

See the information here:
https://www.astro.princeton.edu/docs/SSH

Make an SSH key:
https://help.github.com/articles/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent/#platform-linux

We strongly recommend that you read these instructions to get a better understanding of ssh and ssh keys. In the following, we will see how to setup an ssh key and use it to. Connect remotely.

### SSH key

SSH keys are a mean of identification between two machines. One machine, the one we would like to ssh from, has to generate a key. The key is then passed on to the machines that we want to access. This way, it ensures that only the machines to which we sent a key can connect.

To setup an ssh key, simply type on your computer:

	ssh-keygen -t rsa

You should the following lines appear:

	Generating public/private rsa key pair.
	Enter file in which to save the key (/Users/remy/.ssh/id_rsa):

This is asking you to chose a file to save the key. The default choice is perfectly fine, so just type `enter`. 

CAREFUL NOW!! The next line is more important. You should now see this appear:
	
	Enter passphrase (empty for no passphrase):

At this point, you NEED to type a passphrase that you will be able to remember. You will be asked to enter your passphrase twice, and something like this will appear:

	Your identification has been saved in /Users/remy/.ssh/id_rsa.
	Your public key has been saved in /Users/remy/.ssh/id_rsa.pub.
	The key fingerprint is:
	SHA256:Vg1VEjLylkGjaOcMje5N9rPWQ3oHMe/1OPjdbUFgVQ remy@macbook-pro.lan
	The key's randomart image is:
	+---[RSA 2048]----+
	|        . +ooB++E|
	|        .o**O. =+|
	|         oo+.=o.+|
	|         .  +.* .|
	|        S  o * =o|
	|       .    + .0=|
	|              .o+|
	|               oo|
	|                 |
	+----[SHA256]-----+

This has created a private key (/Users/remy/.ssh/id_rsa) that you should keep like it is your browser history. The public key (/Users/remy/.ssh/id_rsa.pub) is made to be shared with computers that would like to connect to. 

To make sure that no one else but you can see these, enter the following lines:
	
	chmod 700 ~/.ssh
	chmod 600 ~/.ssh/id_rsa

For this purpose, send your public key to yourself via email for instance or dropbox so that you can have it on your department machine.

On your department machine, go to the folder where your `id_rsa.pub` is. We will now append our authorized_keys file to allow the key to be used on your laptop:

	cat id_rsa.pub >> ~/.ssh/authorized_keys

Again, let's make sure that our files remain visible only by us:

	chmod 700 ~/.ssh
	chmod 600 ~/.ssh/authorized_keys


Now you should be able to ssh your department machine from your computer.
Type:
	
	ssh login@domain 

It should actually look like this:
	
	ssh remy@coma.astro.princeton.edu
or:
	ssh remy@leto.astro.princeton.edu

(Replace login and domain by the appropriate items) There you go, you are now using your department machine from your laptop.

### SCP

A particularly useful command using the ssh protocol is the scp command. Command scp serves the same purpose as cp, but between remote machines (your laptop and your department machine for instance). The scp command is used as follows:

	scp origin user@host:path

Create a file on your machine with text,not on the department machine, so this should not follow the ssh command. Either terminate ssh or open a new terminal.

	cat > your_filename.txt
	your text
	ctrl-c

Now copy the file to your department machine:

	scp ./your_filename.txt user@host:

Try the other way around: copying a file from your department machine to your laptop.


