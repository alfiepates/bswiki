/*
Title: rTorrent SCGI RPC
Description: Use your own rTorrent front end
*/

What follows is a script for installing and configuring [nginx](http://nginx.org) to be an authenticating SSL proxy for rTorrent. This lets you talk directly to rTorrent from a remote machine. 

(I use this for Transdroid, but I'm sure there are other clients; you might even be able to run your own local ruTorrent.) 

The following is a shell script that you could copy and paste into a file on your BaconSeed seedbox, for instance by typing: 

`cat > install_nginx.sh` 

and then pasting, and saving with Ctrl-D. You would then run the script with 

`sh install_nginx.sh`

 **nginx installation/configuration shell script** 
 ```#!/bin/bash
  # Get the source echo "FETCHING SOURCE..."
  cd ~ mkdir -p -v ~/nginx/src || exit 
  cd ~/nginx/src || exit 
  wget http://nginx.org/download/nginx-1.0.12.tar.gz > /dev/null 
  tar xzf nginx-1.0.12.tar.gz || exit 
  cd nginx-1.0.12 || exit
  
  # Compile and install (we want ssl, but we don't want rewrite since it depends on libpcre) 
  echo "COMPILING..." 
  ./configure --prefix="$HOME/nginx" --user="$USER" --group="$USER" --with-http_ssl_module --without-http_rewrite_module > /dev/null 
  make > /dev/null make install
  
  # Now we generate the necessary config 
  echo "CONFIGURING..." 
  if [[ ! -e ~/nginx/conf ]]; then 
  	exit 
  fi 
  # Configure 
  cd ~/nginx/conf
  
  # A quick self-signed SSL cert 
  openssl req -x509 -nodes -days 3650 -subj "/CN=$HOSTNAME/O=Baconseed" -newkey rsa:1024 -keyout $HOSTNAME.key -out $HOSTNAME.crt chmod 600 $HOSTNAME.key
  
  # And then the actual nginx.conf file. 
  # The important bit is scgi_pass, which points us at the rtorrent SCGI socket 
  LISTEN_PORT=$(($(id -u)+5000)) 
  SCGI_FOLDER=`grep rpc.socket ~/public_html/rutorrent/conf/config.php | grep '^\s*\$scgi_host' | sed -e 's/.*\.config\/rtorrent\///' -e 's/\/rpc\.socket.*//'`
  echo " worker_processes 1; events {   worker_connections 512; }
  http {
    server {
        listen $LISTEN_PORT default_server ssl;     
        ssl_certificate \"$HOME/nginx/conf/$HOSTNAME.crt\";     
        ssl_certificate_key \"$HOME/nginx/conf/$HOSTNAME.key\";     
        server_name $HOSTNAME;     auth_basic \"Baconseed rTorrent\";     
        auth_basic_user_file \"$HOME/public_html/rutorrent/.htpasswd\";     
        
        location / {       
            scgi_pass \"unix:$HOME/.config/rtorrent/$SCGI_FOLDER/rpc.socket\";          }   
    } 
} " > ~/nginx/conf/nginx.conf
  echo "DONE!" 
  echo "" 
  echo "You can start nginx with:" 
  echo " ~/nginx/sbin/nginx" 
  echo "" 
  echo "To stop:" 
  echo " ~/nginx/sbin/nginx -s stop" 
  echo "" 
  echo "Your server will run on port $LISTEN_PORT" 
  echo ""```
  
**Okay, what did you just do?**

1.  Download and extract the nginx source (from http://nginx.org/download/nginx-1.0.12.tar.gz)
2.  Compile nginx with SSL but without the (missing) rewrite module: `./configure --prefix="$HOME/nginx" --user="$USER" --group="$USER" --with-http_ssl_module --without-http_rewrite_module > /dev/null`
3.  Install nginx into your home folder: `make && make install`
4.  Since we want SSL, we need SSL certificates. The easiest thing is to make a self-signed one for the server we're on: `openssl req -x509 -nodes -days 3650 -subj "/CN=$HOSTNAME/O=Baconseed" -newkey rsa:1024 -keyout $HOSTNAME.key -out $HOSTNAME.crt`
5.  Then the nginx.conf. We enable SSL, HTTP Basic Auth (using the same password file as ruTorrent), and `scgi_pass` to forward connections to rTorrent