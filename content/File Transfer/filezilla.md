/*
Title: Connecting with FileZilla
Description: A brief walkthrough on how to configure FileZilla to connect to your server.
*/

FileZilla is a popular and open source FTP/SFTP client under constant development. This quick guide should help you get set up easily. **1\. Download FileZilla** The first step is to download the client to your computer. You can get the files here: [FileZilla - Client Download Page](http://filezilla-project.org/download.php?type=client). Choosing which file to get is up to you and your operating system. If you are on Windows, I recommend downloading and running the installer unless you are installing this on a flash drive or portable device. In that case, you'd want the zip. **2\. Setting Up The Connection** The next step is to open FileZilla and set up your connection. Follow the steps below in the order presented.

1.  Go to File > Site Manager.![Site Manager](http://i.imgur.com/7k6wAgh.png)
2.  Next, you're going to need to tell some of your user info to FileZilla.![Site Manager Settings](http://i.imgur.com/56mzZq4.png)
    1.  On the left hand side clickl 'New Site' and add whatever name you like
    2.  In the host field, you want to put in the server address (spirit.baconseed.org, grace.baconseed.org, etc.) . For the protocol, you can either choose FTP or SFTP, since BaconSeed provides you with both. We recommend using SFTP as all incoming and outgoing traffic is encrypted, making the connection more secure.
    3.  You can leave the Port box blank.
    4.  For Logon Type, you want to select Normal.
    5.  Your username is the user you decided to create with BaconSeed.
    6.  Your password is the password you decided to create with BaconSeed
    7.  You can either leave the Comments text area blank, or add in some helpful comments to remind you of what it is. It really doesn't matter.
3.  Connect To Your Server ![Connected!](http://i.imgur.com/77jg9Uy.png)NOTE: you MAY first be presented with the following dialog: ![Host Key](http://i.imgur.com/ZxnzByk.png)If so, it is typically recommended to accept it and to use the 'Always Trust...' option. You should be connected now and see a screen like the above. You can either use the right panel and the left panel together to drag and drop to specific directories, or, the easier method is to just drag and drop from wherever you want! So you can drag something from your desktop into the right pane of FileZilla and it will upload it to the server. Doesn't have to be your desktop either. This also applies from dragging something from your server to your computer.

You're all done! Wasn't that easy?