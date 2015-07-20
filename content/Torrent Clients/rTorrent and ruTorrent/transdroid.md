/*
Title: rTorrent + Transdroid
Description: How to set up Transdroid for usage on your slot or server
*/

Transdroid is the best (and only) torrent interface for rTorrent (and Deluge) on Android. It allows you to control, and add, torrents from your phone.

## Setup

Setting up Transdroid is fairly simple:

*   Download Transdroid [here](http://transdroid.org/latest)
*   Install the .apk to your phone or tablet
*   Open up Transdroid, and go to Settings
*   Click Add new server
*   Give the new server a name (Baconseed for example)
*   Server type is rTorrent
*   IP or domain name is your slot server (e.g. gemini.baconseed.org, mercury.baconseed.org)
*   Port: 443
*   Select Use authentication, and fill in your username and password you use to access your rutorrent page
*   SCGI folder: /~USERNAME/rutorrent/plugins/httprpc/action.php
*   For Advanced settings, you can choose what you want or need, but you need to enable SSL

That's it, Transdroid should now work, and display your torrents.