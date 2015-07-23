/*
Title: Setting up your SSH key for passwordless SSH
Description: cakewizard shows you how to configure your SSH for passwordless authentication
*/

#Setting up your SSH key for passwordless SSH.

Using an SSH key allows you to login to your server in a more secure fashion than using a password: While the average password could eventually be bruteforced, attacking an SSH key is pretty much impossible, at least in our lifetimes.

Key-based SSH login works using two keys: A private key, which you hold, and a public key, which you place on the server. The two are mathematically related in such a way that the server can give you its public key and ask your computer to compute a "signature" from the two keys, while it does the same with your public key and its own private key. If the two signatures match up, the server can be confident that you are in fact you, and allow you access to the server without asking for a password. This all happens in a second or so, which makes it more convenient in addition to being way more secure.

##Linux

###Generating and storing your SSH keypair.

This article assumes that you haven't used key-based SSH before, and therefore haven't generated a key. If you have, skip to the next step.

Otherwise, fire this command to generate your keypair:

    ssh-keygen -t rsa

After a moment, you'll be greeted with the following prompt.

    Enter file in which to save the key (/home/tutorial/.ssh/id_rsa):

Just hit enter, it's already pointing at the right place; the key will be saved in `~/.ssh/id_rsa`, so that `ssh` will be able to find it when making a connection.

`ssh-keygen` will then ask you if you want to secure the key with a passphrase:

    Enter passphrase (empty for no passphrase):
    Enter same passphrase again:

Adding a passphrase will increase the security of the key by ensuring that even if the keyfile falls into the wrong hands, the key isn't accessable by malicious parties. However, a passphrase will need to be entered every time the key is used to login, somewhat negating the convenience of passwordless SSH. Note that the security of the SSH connection itself isn't compromised by the lack of a passphrase: it's perfectly fine to hit enter here and have no passphrase provided you're confident in the security of the key storage.

If you want a passphrase, enter it at both passphrase prompts and then that's it! Your key is generated and ready for use. The whole process looks a bit like this:

    tutorial@tutorial:~$ ssh-keygen -t rsa
    Generating public/private rsa key pair.
    Enter file in which to save the key (/home/tutorial/.ssh/id_rsa):
    Created directory '/home/tutorial/.ssh'.
    Enter passphrase (empty for no passphrase):
    Enter same passphrase again:
    Your identification has been saved in /home/tutorial/.ssh/id_rsa.
    Your public key has been saved in /home/tutorial/.ssh/id_rsa.pub.
    The key fingerprint is:
    7d:90:cf:43:43:2f:b8:7a:04:4c:be:57:6a:e3:2e:99 tutorial@tutorial
    The key's randomart image is:
    +---[RSA 2048]----+
    |        .   .    |
    |       +   + .   |
    |        + + = .  |
    |         + O o   |
    |        S O =    |
    |         * o .   |
    |        .oo      |
    |        Eo       |
    |         ..      |
    +-----------------+

###Copying the public key to the server

Now that you have a keypair, you need to give the server your public key. There are two ways of doing this, using `ssh-copy-id` and transferring the keys using some SSH wizardry.

The first method is super simple. Let's assume that `tutorialserver.baconseed.org` is the hostname of your server, and `tutorial` is your username on the server. Execute the following command to copy across your keys.

    ssh-copy-id tutorial@tutorialserver.baconseed.org

There, that was easy!

The second method is slightly more complex, but it's a nice way to practice your terminal skills.

    cat ~/.ssh/id_rsa.pub | ssh tutorial@tutorialserver.baconseed.org "mkdir -p ~/.ssh && cat >>  ~/.ssh/authorized_keys"

You should see something like this:

    The authenticity of host 'tutorialserver.baconseed.org' can't be established.
    RSA key fingerprint is b1:2d:33:67:ce:35:4d:5f:f3:a8:cd:c0:c4:48:86:12.
    Are you sure you want to continue connecting (yes/no)? yes
    Warning: Permanently added 'tutorialserver.baconseed.org' (RSA) to the list of known hosts.
    tutorial@tutorialserver.baconseed.org's password:
    Now try logging into the machine, with "ssh 'tutorial@tutorialserver.baconseed.org'", and check in:

    ~/.ssh/authorized_keys

    to make sure we haven't added extra keys that you weren't expecting.

###Logging in using passwordless SSH

Now, the moment you've been waiting for; time to actually log in! Execute the following;

    ssh tutorial@tutorialserver.baconseed.org

If you've set a passphrase, it'll ask for it now.

Ta-da! You're in! You've enabled passwordless SSH, you can now log into your server without dealing with passwords, safe in the knowledge that your connection is safe and secure.

##Windows

###Downloading and installing PuTTY and PuTTYgen
Windows doesn't come with a built-in SSH client, so you're going to need to download one. PuTTY is by far the most widely-used Windows SSH client, so we'll use that. You're also going to need PuTTYgen, which is PuTTY's equivalent to `ssh-keygen`. Download PuTTY.exe and PuTTYgen.exe from  [here](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html), then place the two executables somewhere easy to find, like your desktop.

![Installing putty](img/sshkeys/installingputty.png)

###Generating and storing your SSH keypair

Launch PuTTYgen.exe. You'll be greeted by the following window:

![PuTTYgen](img/sshkeys/puttygen.png)

The default parameters work great in most cases, but if you want a bit more security then you can bump up the number of bits to 4096.

Once you're happy with the settings, hit the generate button and wiggle your mouse around in the blank rectangle to generate some this randomness, or _entropy._ This makes sure your key is randomly generated in such a fashion that shouldn't be reproducable by others.

![PuTTYgen](img/sshkeys/generatingentropy.png)

Once the key's been generated, you'll be presented with the following:

![PuTTYgen](img/sshkeys/generatedkey.png)

You can optionally add a passphrase to the key, as described in the Linux section of this article. For the sake of demonstration we'll leave it out; in most cases that's absolutely fine.

Click the "Save public key" and "Save private key" buttons, and select a secure location to store them. For the tutorial, we'll put them on the desktop for easy access but this is a bad idea. It's best to store them on something like a USB drive which you can remove from the computer and take with you.

Give the public key the extension `.txt`. PuTTYgen will save the private key as `.ppk`.

Once you've done this, you can now close the window, your key is generated.

###Copying the public key to the server

Fire up PuTTY.exe.

![PuTTY](img/sshkeys/putty.png)

Input the hostname or IP address of your server, and leave everything else as is. Click "open", and you'll be greeted by the following prompt.

![PuTTY RSA Key Warning](img/sshkeys/puttywarning.png)

This is because PuTTY doesn't recognise your server, having never connected to it before. Hit "yes."

You'll then be offered a login prompt. Enter your server username and password here.

![SSH login prompt](img/sshkeys/puttylogin.png)

Run the following command:

    mkdir -p ~/.ssh && touch ~/.ssh/authorized_keys && nano ~/.ssh/authorized_keys

This opens the `authorized_keys` file in `nano`, a basic text editor.

![Nano with authorized_keys open](img/sshkeys/authorizedkeysnano.png)

Now, open PuTTYgen.exe and load the private key (the `.ppk` file) you generated earlier.

Select all the text in the box at the top, right-click, and hit "copy".

![Copying your public key](img/sshkeys/copypublickey.png)

Return to PuTTY, and right-click. `nano` should look something like this:

![Nano with authorized_keys open](img/sshkeys/authorizedkeysnanopaste.png)

Hit `CTRL+X`, then `Y`, then hit enter to save the file.

![Nano with authorized_keys open](img/sshkeys/authorizedkeysnanosave.png)

You'll be dropped back into the terminal prompt. Type `logout` to exit your SSH session. Your public key is now on the server.

###Configuring PuTTY to use passwordless SSH

Launch PuTTY.exe again.

Scroll down to "Connection", click "Data", then enter your username.

![Entering your username](img/sshkeys/enterusername.png)

Expand "SSH" and then click "Auth". Click the "Browse" button under "Private key file for authentication", and select the `.ppk` file you generated earlier.

![Selecting the private key](img/sshkeys/puttyprivatekey.png)

Scroll back up to "Session", enter your server's hostname or IP, give the session a name, and then hit save.

![Saving your session](img/sshkeys/savingsession.png)

That's it, you're done! Hit "open" to test the connection. If everything went right, you should be dropped into your server's terminal.

![The end result](img/sshkeys/puttyloggedin.png)
