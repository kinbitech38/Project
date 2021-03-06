#  LAMP STACK
## update a list of packages in package manager 
       'sudo apt update'
## run apache2 package installation 
    'sudo apt install apache2'
## To verify that apache2 is running as a service in our os, use the following command 
    'sudo systemclt status apache2'


## Use ‘apt’ to acquire and install this software
    '$ sudo apt install mysql-server'
## When prompted, confirm installation by typing Y, and then ENTER.

## When the installation is finished, log in to the MySQL console by typing
    '$ sudo mysql'
## Start the interactive script by running
    '$ sudo mysql_secure_installation'
## When you’re finished, test if you’re able to log in to the MySQL console by typing
     '$ sudo mysql -p'
## To exit the MySQL console, type:
      'mysql> exit'


## To install these 3 packages at once, run:
       'sudo apt install php libapache2-mod-php php-mysql'
## Once the installation is finished, you can run the following command to confirm your PHP version
        'php -v'


## To create the directory for projectlamp using "mkdir" command s as follows.
     'sudo mkdir /var/www/projectlampsudo'
## To assign ownership of the directory with your current system user.
      'sudo chown -R $USER:$USER /var/www/projectlamp'
## To create and open a new configuration file in apache's site- available directory using your preferred command-line editor
      'sudo vi /etc/apache2/sites-available/projectlamp.conf'
## Create a new blank file. Paste in the following bare-bones configuration by hitting on 'i' on the keyboard to enter the insert mode, and paste the text.
    '<VirtualHost *:80>
    ServerName projectlamp
    ServerAlias www.projectlamp 
    ServerAdmin webmaster@localhost
    DocumentRoot /var/www/projectlamp
    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined 
     </VirtualHost>'

## To save and close the file, simply follow the steps below:
     1.Hit the esc button on the keyboard
     2.Type :
     3.Type wq. w for write and q for quit
     4.Hit ENTER to save the file.
## You can use the ls command to show the new file in the sites-available directory
      'sudo ls /etc/apache2/sites-available'
## You can now use a2ensite command to enable the new virtual host
       'sudo a2ensite projectlamp'
## To disable Apache’s default website use a2dissite command , type
       'sudo a2dissite 000-default'
## To make sure your configuration file doesn’t contain syntax errors, run
        'sudo apache2ctl configtest'
## Finally, reload Apache so these changes take effect
        'sudo systemctl reload apache2'
## Create an index.html file in that location so that we can test that the virtual host works as expected
        'sudo echo 'Hello LAMP from hostname' $(curl -s http://169.254.169.254/latest/meta-data/public-hostname) 'with public IP' $(curl -s http://169.254.169.254/latest/meta-data/public-ipv4) > /var/www/projectlamp/index.html'
##  Now go to your browser and try to open your website URL using IP address  .....


## In case you want to change this behavior, you’ll need to edit the /etc/apache2/mods-enabled/dir.conf file and change the order in which the index.php file is listed within the DirectoryIndex directive
       'sudo vim /etc/apache2/mods-enabled/dir.conf'
       '<IfModule mod_dir.c>
        #Change this:
        #DirectoryIndex index.html index.cgi index.pl index.php index.xhtml index.htm
        #To this:
        DirectoryIndex index.php index.html index.cgi index.pl index.xhtml index.htm
        </IfModule>'
## After saving and closing the file, you will need to reload Apache so the changes take effect:
        'sudo systemctl reload apache2'
##   Create a new file named index.php inside your custom web root folder:
        'vim /var/www/projectlamp/index.php'
## This will open a blank file. Add the following text, which is valid PHP code, inside the file:
      '<?php'
      ' phpinfo();'
## After checking the relevant information about your PHP server through that page, it’s best to remove the file you created as it contains sensitive information about your PHP environment -and your Ubuntu server. You can use rm to do so:
       'sudo rm /var/www/projectlamp/index.php'
    
      