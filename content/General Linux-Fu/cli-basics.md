/*
Title: Command Line (CLI) Basics
Description: fiver22's brief introduction on how to look like a complete hacker, win friends, and influence people.
*/



#### New to the CLI (Command Line Interface)?

##### Below are some basic and common commands that you can use with your baconseed slot or box:

First let's ssh into the box. On your PC, open a terminal(*nix, mac) or use putty(win). In the terminal type the following, filling in YOUR info:

1.  `ssh YourUserName@YourServer.baconseed.org` hit Enter/Return
2.  you will be prompted for your password. You will NOT see the password being typed in. Hit enter/return
3.  you are now logged into the seedboxwhich will look like:

`YourUsername@YourServer:~$` you are now at the prompt and can type commands try them out: after a command hit enter/return

*   "ls" -list files in the directory where you are
*   "ls -a" -list all files (including hidden ones -hidden files start with ".") in the directory

"cd" -change directory ex:

*   `cd Downloads` will put you in a folder called 'Downloads' (if it exists)
*   `cd Downloads/torrentfiles` will put you in the dir/folder called 'torrentfiles' if it exists inside 'Downloads'
*   `cd ..` will put you up one directory
*   `cd ~`" will bring you to you 'home' directory wherever you are
*   `cd /` will bring you to the 'root' dir (if you're allowed)

"quota" -tells you the space you have and how much you've used.

`quota -s`

<pre>fiver22@gemini:~$ quota -s
Disk quotas for user fiver22 (uid 1006): 
     Filesystem   space   quota   limit   grace   files   quota   limit   grace
       /dev/md1   205G    300G    350G            41423
</pre>

where 'space' is actual space used, 'quota' is the size of the slot, and 'limit' is the hard limit at which point things break "cp" - 'copy' ex:

*   `cp file1 file2` will copy file1 to file2 *BE CAREFUL! IT WILL OVERWRITE file2 IF IT ALREADY EXISTS*

"mv" -move a file/folder(dir) ex:

`mv file1 /home/fiver22/Downloads`

mv also allows you to rename files, so:

`mv fileABC file123`

"rm" -remove a file. "rm -r remove a directory ***BE CAREFUL NOT TO RM SOMETHING IMPORTANT!**

`rm rtorrent.lock`

"mkdir" makes a directory in the directory you are in ex:

`mkdir testdirectory`

"chmod" -I'm not going into this much here, the most common use for it is to make something executable ex:

`chmod +x ThatCoolApp`

"kill" kills (stops) a process -in a GUI the 'x' in the Title Bar of any GUI application.*   `kill PID` -kills the process with whatever PID enteredone of the easiest ways to kill a process is via HTOP;

*   run HTOP in the shell and it will list all the processes running. Highlight it and hit F9 -this defaults to kill -15/sigkill, hit enter and the process should be stopped

"du" lists files and dirs and estimates filesize

*   `du -h` does the same but makes it human readable (ie prints numbers in MiB/GiB)

ex:

*   `du public_html`" lists all the files in 'public_html' and estimales their total size -in MY case it ends with the line '7913832 public_html/' which is next to useless to ME. Instead, if I do "du -h public_html" the final line will read '7.6G public_html/' -a much easier to understand number.

making 'du' more useful:

*   `du -h --max-depth=1` lists ONLY dirs in the dir you are in, and shows size
*   `du -h --max-depth=1 | sort -h` same as above but lists in order of size

"htop" shows you an active list of the running processes on your slot.

##### rsync instead of (s)ftp:

There are many instances where rsync might be prefered over ftp -for ex: downloading from my seedbox to my home via ftp just won't max out my connection -especially when grabbing one file at a time. rsync allows me to accomplish this much more quickly. An example of the rsync command I use to pull FROM my seedbox to my home pc (ignore "quotes"): `rsync -hrP fiver22@gemini.baconseed.org:/mnt/disk1/fiver22/torrents/torsync/ /home/fiver22/Downloads` What does all that mean? In a nutshell it says go to my computer: go to my baconseed dir called 'torsync', grab anything that's in there and pull it down to my computer and put it in the 'Downloads' dir.

*   "rsync" is simply the program you invoke
*   "-hrP" are triggers for things like human readable output, recursive, and progress
*   "yourusername" should be self evident (it's your username, after-all)
*   "@someaddress.baconseed.org:" is the address you use to access your seedbox (or if you're going in the other direction, it's the ip/address of your home pc)
*   "/dir/dir/etc..." is the path to the directory you want to copy -wherever your data is
*   the second "/dir/dir/etc..." is the path to where you'll put the data
*   hit enter(return) and it'll likely prompt you for your password at baconseed(or your home pc), then you'll see the data begin to transfer.

##### Exiting the seedbox.

You can either type:

*   `logout` and hit enter OR
*   ctrl+d to disconnect.

##### NOTES:

*   watch your spelling. No, SERIOUSLY, watch your spelling.
*   caSe maTteRs: /downloads, /Downloads, /dowNloads are 3 DIFFERENT dirs/folders
*   the term "directory" and "folder" are interchangeable, in the *nix world people tend to use "directory" or 'dir'
*   when you open a terminal, or log into a seedbox, you almost always are going to start in the '~' directory which you can think of as 'home' On baconSeed the structure may be FULLY expressed as /mnt/disk#/YourUserName -you can use the pwd command to see the directory structure. 'pwd' is Print Working Directory' and you can use 'pwd' wherever you are in the shell.

<pre>/
 mnt/
     disk#/
           YourUserName/
                         .bash_history  .bash_logout  .bashrc  .config  /downloads  .profile  /public_html .rtorrent.rc  .viminfo  /watch
</pre>

**welcome**