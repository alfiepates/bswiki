/*
Title: lftp - segmented downloading
Description: get better transfer speeds with segmented downloading.
*/

### lftp for *nix

##### Installation

You'll want to install it first: for Debian-based distros (Debian, Ubuntu, Linux Mint, Crunchbang, etc.) it's almost certainly in your repos.

*   As root `apt-get install lftp`

You can also grab the source and/or binaries from the [official lftp site](http://lftp.yar.ru/)

##### Usage

*   you can launch lftp by running the command `lftp` in a terminal,

you'll note the prompt changes to `[ftp] :~>`

##### Connecting to baconseed

The proper way to connect to your baconSeed account with lftp is to issue the command

*   `lftp sftp://YourUserName@YourServer.baconseed.org`

<sup>ex:</sup> ![](http://i.imgur.com/zfahiLx.png)  
<small>Note: Your local directory, by default, is the directory you launched lftp from.</small>  

##### Navigating in lftp

For information on how to get around your directories and files you can reference common bash commands (cd, ls, etc) found in <small>[Getting Started with the CLI](https://www.baconseed.org/node/57)</small>  

##### Transferring Files

Transfers are done using the commands `pget` for files, and `mirror` for directories(folders).

For the sake of simplicity we will assume that you wish to use segmented downloading which often allows for faster transfers. Another assumption you will see is the choice of 5 segments. The number can be altered, of course.

*   To transfer a single file `pget -n 5 FileName`
*   To transfer a directory `mirror --use-pget-n=5 DirectoryName`

<sup>ex:</sup>![](http://i.imgur.com/UXNzNRs.png)