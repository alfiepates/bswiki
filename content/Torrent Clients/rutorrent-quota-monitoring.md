/*
Title: Disk Quota Monitoring in ruTorrent
Description: Get an accurate disk quota in the disk plugin, refreshed every five minutes.
*/

**Set up quota monitoring by hand:**

1.  Edit `~/public_html/rutorrent/plugins/diskspace/action.php`, and replace it with:  
     `  <?php     require_once( "../../php/util.php" );     $quota_txt = `cat ~$USER/public_html/quota.txt`;     $line = split("\n", $quota_txt);     $fields = codeg_split('/\s+/', $line[2]);     cachedEcho('{ "total": '.($fields[3]*1024).', "free": '.(($fields[3]-$fields[2])*1024).' }',"application/json");   ?>` Where `$USER` is your username
2.  Create the quota monitoring script; it just calls `quota` and writes the output to a file. I called mine `update_quota.sh` in my home folder:  
     `  #!/bin/bash   quota -w -f /mnt/disk1 --show-mntpoint > ~/public_html/quota.txt` And make it executable: `chmod 744 ~/public_html/update_quota.sh`
3.  Add an entry to your crontab to run the script every five minutes:  
     `  */5 *  *   *   *     /mnt/disk1/$USER/update_quota.sh` (Replace `$USER` with your username. To edit your crontab (with, say, vim): `EDITOR=vim crontab -e`.
4.  Enjoy up-to-the-5-minute monitoring your quota in ruTorrent!

-----   
**Waah, I am lazy! Do it for me!**  
 _(Okay, fine. Put this in a script and run it.)_   
```
#!/bin/bash 
echo "Updating plugins/diskspace/action.php" 
mv -v ~/public_html/rutorrent/plugins/diskspace/action.php 
~/public_html/rutorrent/plugins/diskspace/action.php.bak 
echo '<?php         require_once( "../../php/util.php" );
$quota_txt = `cat ~'"$USER"'/public_html/quota.txt`;
$line = split("\n", $quota_txt);
$fields = codeg_split('\''/\s+/'\'', $line[2]);
cachedEcho('\''{ "total": '\''.($fields[3]*1024).'\'', "free": '\''.(($fields[3]-$fields[2])*1024).'\'' }'\'',"application/json"); ?>' > ~/public_html/rutorrent/plugins/diskspace/action.php 
echo "Creating update_quota.sh in your home folder" 
echo '#!/bin/bash quota -w -f --show-mntpoint > ~/public_html/quota.txt' > ~/update_quota.sh 
chmod 744 ~/update_quota.sh 
echo "Updating crontab" 
crontab -l | grep -v update_quota\\.sh > .old_crontab 
echo "*/5 * * * * /mnt/disk1/$USER/update_quota.sh" >> .old_crontab 
crontab .old_crontab 
rm .old_crontab 
echo "Running update_quota.sh once so you don't have to wait five minutes." 
~/update_quota.sh 
echo "All done!"```
