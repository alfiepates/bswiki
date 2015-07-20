/*
Title: ruTorrent Overview
Description: A brief introduction to the ruTorrent web UI
*/

The dynamic pair of rTorrent and ruTorrent make one heck of a client, for users of all experience levels. The rTorrent program runs on the server, in the background, all the time, while the ruTorrent script allows you to control rTorrent from your browser. Let's examine the two pieces separately.

## ruTorrent

An opens-source project written mostly in PHP that allows you to control rTorrent from the comfort of your favorite web browser. If you're familiar with uTorrent on the Windows or Mac desktop, or with Deluge GTK, this will seem pretty comfortable. [![](http://i.imgur.com/53fKG.png "Hosted by imgur.com")](http://imgur.com/53fKG)The left menu offers sorting, including by status, tracker, and custom-defined labels. The disk graph in the bottom right is a reflection of how much disk quota you have used or have left. Here's a quick explanation of the icons in the menu bar and what they do:

<table>

<tbody>

<tr>

<td>[![](http://i.imgur.com/NVX6P.png "Hosted by imgur.com")](http://imgur.com/NVX6P)</td>

<td>"Add Torrent" - allows you to add .torrent files from a directory on the seedbox, by uploading from your local computer, or from a download URL.</td>

</tr>

<tr>

<td>[![](http://i.imgur.com/Q3hDs.png "Hosted by imgur.com")](http://imgur.com/Q3hDs)</td>

<td>"Create Torrent" - exactly what it says on the tin. See the article on creating and upload torrents for more information.</td>

</tr>

<tr>

<td>[![](http://i.imgur.com/6MoHu.png "Hosted by imgur.com")](http://imgur.com/6MoHu)</td>

<td>"Delete" - deletes the selected torrent.</td>

</tr>

<tr>

<td>[![](http://i.imgur.com/vedST.png "Hosted by imgur.com")](http://imgur.com/vedST)</td>

<td>"Start" - starts the selected torrent.</td>

</tr>

<tr>

<td>[![](http://i.imgur.com/rfAxz.png "Hosted by imgur.com")](http://imgur.com/rfAxz)</td>

<td>"Pause" - pauses the selected torrent.</td>

</tr>

<tr>

<td>[![](http://i.imgur.com/Am7jL.png "Hosted by imgur.com")](http://imgur.com/Am7jL)</td>

<td>"Stop" - stops the selected torrent.</td>

</tr>

<tr>

<td>[![](http://i.imgur.com/2oQ6e.png "Hosted by imgur.com")](http://imgur.com/2oQ6e)</td>

<td>"RSS" - allows you to add RSS feeds and rules to automatically download content from a site RSS feed.</td>

</tr>

<tr>

<td>[![](http://i.imgur.com/GLPpZ.png "Hosted by imgur.com")](http://imgur.com/GLPpZ)</td>

<td>"Settings" - controls various settings about rTorrent and ruTorrent."</td>

</tr>

</tbody>

</table>

## rTorrent

An open-source project comprised of libTorrent and the rTorrent client itself, rTorrent is a standard for most seedbox clients as it is stable, reliable, and features an XML-RPC interface when compiled correctly. The BaconSeed rTorrent comes with a patch that adds a green color to torrents that are completed and red to torrents that are still not 100% downloaded. **Starting and Stopping rTorrent - and what is this "screen" thing?** When you get your slot provisioned, rTorrent should already be started up for you. The rTorrent program itself runs as your user on the server. Due to the way *nix works, processes that are started as the user generally die as soon as you log out - that's why we start rTorrent inside a "screen." This simple utility allows you to put a process (like rTorrent) into the background, making it run even when you're not connected to the server on the command line. To "attach" to a screen means to bring the existing screen and associated processes up from the background and into your terminal. "Detaching" from screen means sending them back into the ether to live until you re-attach. So, to attach to a screen, we use

<pre>$: screen -Udr</pre>

Once inside screen, you can detach from it by pressing Ctrl+a d (that's Ctrl+a at the same time, then let those go and press the d key). For more information on screen, read the wiki article by the same name. **Controlling rTorrent from the command line** Controlling rTorrent from the CLI can be a powerful troubleshooting tool, especially if ruTorrent is buggy or rTorrent is under high load. It does take some getting used to, but is pretty simple to learn. To get started, you'll need to first re-attach to the screen rTorrent is running in (see above section). You should now see some sort of interface. Our example has a debian ISO torrent loaded. Here's what that looks like when the torrent is paused, and hasn't downloaded or uploaded anything: [![rTorrent with debian ISO loaded](http://i.imgur.com/g9Ybl.png "Hosted by imgur.com")](http://imgur.com/g9Ybl)That all looks pretty complicated. Let's take a look at a few pieces, starting with the menu bar at the bottom. [![Throttle](http://i.imgur.com/7rBAa.png "Hosted by imgur.com")](http://imgur.com/7rBAa)Throttle refers to speed limits. You don't need to tweak this value on baconSeed, as our servers handle this automatically. rTorrent notates all speeds as upload / download. [![](http://i.imgur.com/AS8sM.png "Hosted by imgur.com")](http://imgur.com/AS8sM)Rate refers to the speeds you are currently getting. Again, rTorrent notates all speeds as upload / download. [![](http://i.imgur.com/NxodK.png "Hosted by imgur.com")](http://imgur.com/NxodK)Port is self explanatory - it is the port rTorrent is currently bound to. Please do not change this port on your baconSeed slot, as it will likely interfere with another customer's client. If you need to change your port for some reason, just contact support. [![](http://i.imgur.com/RZTl5.png "Hosted by imgur.com")](http://imgur.com/RZTl5)The "U" refers to the number of active download slots. Right now, our example client is using zero out of an unlimited number, which rTorrent displays as zero. As you seed more things actively, you'll see this number go up. The "D" refers similarly to the number of active download slots. H and S are more advanced and beyond the scope of this article - they have to do with protocol-related items that the average user won't need to understand. The "F" represents the number of open files. rTorrent will automatically open and close files as needed to maintain the best seeding spread. **Keyboard Controls** You can control rTorrent completely from the command line - here's a quick primer of frequently used keyboard commands for doing so.

<table>

<tbody>

<tr>

<td>backspace</td>

<td>Add torrent using an URL or file path. Use tab to view directory content and do auto-complete. Also, wildcards can be used. For example: ~/torrent/*</td>

</tr>

<tr>

<td>return</td>

<td>Same as backspace, except the torrent remains inactive. (Use <tt>^</tt>s to activate)</td>

</tr>

<tr>

<td><tt>^</tt>o</td>

<td>Set new download directory for selected torrent. Only works if torrent has not yet been activated.</td>

</tr>

<tr>

<td><tt>^</tt>s</td>

<td>Start download. Runs hash first unless already done.</td>

</tr>

<tr>

<td><tt>^</tt>d</td>

<td>Stop an active download, or remove a stopped download.</td>

</tr>

<tr>

<td><tt>^</tt>k</td>

<td>Stop and close the files of an active download.</td>

</tr>

<tr>

<td><tt>^</tt>r</td>

<td>Initiate hash check of torrent. Without starting to download/upload.</td>

</tr>

</tbody>

</table>

For more information on other keyboard controls and all the other screens, check out http://community.rutorrent.org/UserInterface.