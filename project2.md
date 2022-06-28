  ## LEMP STACK IMPLEMENTATION 

## Since this is our first time using apt for this session, start off by updating your server’s package index. Following that, you can use apt install to get Nginx installed:
        'sudo apt update'
        'sudo apt install nginx'
## To verify that nginx was successfully installed and is running as a service in Ubuntu, run
        'sudo systemctl status nginx'
## First, let us try to check how we can access it locally in our Ubuntu shell, run:
        'curl http://localhost:80'
               or
        'curl http://127.0.0.1:80'
## Now it is time for us to test how our Nginx server can respond to requests from the Internet.
## Open a web browser of your choice and try to access following url
        'http://<Public-IP-Address>:80'
## Another way to retrieve your Public IP address, other than to check it in AWS Web console, is to use following command
        'curl -s http://169.254.169.254/latest/meta-data/public-ipv4'
## The URL in browser shall also work if you do not specify port number since all web browsers use port 80 by default.
## If you see following page, then your web server is now correctly installed and accessible through your firewall.


## Now that you have a web server up and running, you need to install a Database Management System (DBMS) to be able to store and manage data for your site in a relational database. MySQL is a popular relational database management system used within PHP environments, so we will use it in our project.
## Again, use ‘apt’ to acquire and install this software:
      '$ sudo apt install mysql-server'
## When the installation is finished, log in to the MySQL console by typing:
       '$ sudo mysql' 
## This will connect to the MySQL server as the administrative database user root, which is inferred by the use of sudo when running this command.
## Start the interactive script by running
    '$ sudo mysql_secure_installation'
## This will ask if you want to configure the VALIDATE PASSWORD PLUGIN.

## Note: Enabling this feature is something of a judgment call. If enabled, passwords which don’t match the specified criteria will be rejected by MySQL with an error. It is safe to leave validation disabled, but you should always use strong, unique passwords for database credentials.

## Answer Y for yes, or anything else to continue without enabling.

      'VALIDATE PASSWORD PLUGIN can be used to test passwords
      and improve security. It checks the strength of password
      and allows the users to set only those passwords which are
      secure enough. Would you like to setup VALIDATE PASSWORD plugin?
      Press y|Y for Yes, any other key for No:'
## If you answer “yes”, you’ll be asked to select a level of password validation. Keep in mind that if you enter 2 for the strongest level, you will receive errors when attempting to set any password which does not contain numbers, upper and lowercase letters, and special characters, or which is based on common dictionary words.
## When you’re finished, test if you’re able to log in to the MySQL console by typing:
    '$ sudo mysql -p'
## Notice the -p flag in this command, which will prompt you for the password used after changing the root user password.
## To exit the MySQL console, type:
    'mysql> exit'

## You have Nginx installed to serve your content and MySQL installed to store and manage your data. Now you can install PHP to process code and generate dynamic content for the web server.

## While Apache embeds the PHP interpreter in each request, Nginx requires an external program to handle PHP processing and act as a bridge between the PHP interpreter itself and the web server. This allows for a better overall performance in most PHP-based websites, but it requires additional configuration. You’ll need to install php-fpm, which stands for “PHP fastCGI process manager”, and tell Nginx to pass PHP requests to this software for processing. Additionally, you’ll need php-mysql, a PHP module that allows PHP to communicate with MySQL-based databases. Core PHP packages will automatically be installed as dependencies.
## To install these 2 packages at once, run
     'sudo apt install php-fpm php-mysql'
## When prompted, type Y and press ENTER to confirm installation.
## You now have your PHP components installed. Next, you will configure Nginx to use them.


## When using the Nginx web server, we can create server blocks (similar to virtual hosts in Apache) to encapsulate configuration details and host more than one domain on a single server. In this guide, we will use projectLEMP as an example domain name.

## On Ubuntu 20.04, Nginx has one server block enabled by default and is configured to serve documents out of a directory at /var/www/html. While this works well for a single site, it can become difficult to manage if you are hosting multiple sites. Instead of modifying /var/www/html, we’ll create a directory structure within /var/www for the your_domain website, leaving /var/www/html in place as the default directory to be served if a client request does not match any other sites.
## Create the root web directory for your_domain as follows:
       'sudo mkdir /var/www/projectLEMP'
## Next, assign ownership of the directory with the $USER environment variable, which will reference your current system user:
    'sudo chown -R $USER:$USER /var/www/projectLEMP'
## Then, open a new configuration file in Nginx’s sites-available directory using your preferred command-line editor. Here, we’ll use nano:
       'sudo nano /etc/nginx/sites-available/projectLEMP'
## This will create a new blank file. Paste in the following bare-bones configuration:
       '#/etc/nginx/sites-available/projectLEMP
         server {
         listen 80;
         server_name projectLEMP www.projectLEMP;
         root /var/www/projectLEMP;
         index index.html index.htm index.php;
         location / {
         try_files $uri $uri/ =404;
         }
         location ~ \.php$ {
         include snippets/fastcgi-php.conf;
         fastcgi_pass unix:/var/run/php/php8.1-fpm.sock;
         }
         location ~ /\.ht {
         deny all;
         }
         }'
## When you’re done editing, save and close the file. If you’re using nano, you can do so by typing CTRL+X and then y and ENTER to confirm.
## Activate your configuration by linking to the config file from Nginx’s sites-enabled directory:
        'sudo ln -s /etc/nginx/sites-available/projectLEMP /etc/nginx/sites-enabled/'
## This will tell Nginx to use the configuration next time it is reloaded. You can test your configuration for syntax errors by typing:
         'sudo nginx -t'
## You shall see following message:
         nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
         nginx: configuration file /etc/nginx/nginx.conf test is successful
         If any errors are reported, go back to your configuration file to review its contents before continuing.
## We also need to disable default Nginx host that is currently configured to listen on port 80, for this run:
        'sudo unlink /etc/nginx/sites-enabled/default'
## When you are ready, reload Nginx to apply the changes:
        'sudo systemctl reload nginx'
## Your new website is now active, but the web root /var/www/projectLEMP is still empty. Create an index.html file in that location so that we can test that your new server block works as expected:
          'sudo echo 'Hello LEMP from hostname' $(curl -s http://169.254.169.254/latest/meta-data/public-hostname) 'with public IP' $(curl -s http://169.254.169.254/latest/meta-data/public-ipv4) > /var/www/projectLEMP/index.html'
## Now go to your browser and try to open your website URL using IP address:
         'http://<Public-IP-Address>:80'
## If you see the text from ‘echo’ command you wrote to index.html file, then it means your Nginx site is working as expected.
## In the output you will see your server’s public hostname (DNS name) and public IP address. You can also access your website in your browser by public DNS name, not only by IP – try it out, the result must be the same (port is optional).
       'http://<Public-DNS-Name>:80'
## You can leave this file in place as a temporary landing page for your application until you set up an index.php file to replace it. Once you do that, remember to remove or rename the index.html file from your document root, as it would take precedence over an index.php file by default.

## Your LEMP stack should now be completely set up.

## At this point, your LAMP stack is completely installed and fully operational.
## You can test it to validate that Nginx can correctly hand .php files off to your PHP processor.
## You can do this by creating a test PHP file in your document root. Open a new file called info.php within your document root in your text editor:
      'sudo nano /var/www/projectLEMP/info.php'
## Type or paste the following lines into the new file. This is valid PHP code that will return information about your server:
        '<?php'
        'phpinfo();'
## You can now access this page in your web browser by visiting the domain name or public IP address you’ve set up in your Nginx configuration file, followed by /info.php:
        'http://`server_domain_or_IP`/info.php'
## You will see a web page containing detailed information about your server.

## After checking the relevant information about your PHP server through that page, it’s best to remove the file you created as it contains sensitive information about your PHP environment and your Ubuntu server. You can use rm to remove that file:
       'sudo rm /var/www/your_domain/info.php'
## You can always regenerate this file if you need it later.

## In this step you will create a test database (DB) with simple "To do list" and configure access to it, so the Nginx website would be able to query data from the DB and display it.

## At the time of this writing, the native MySQL PHP library mysqlnd doesn’t support caching_sha2_authentication, the default authentication method for MySQL 8. We’ll need to create a new user with the mysql_native_password authentication method in order to be able to connect to the MySQL database from PHP.

## We will create a database named example_database and a user named example_user, but you can replace these names with different values.

## First, connect to the MySQL console using the root account:
        'sudo mysql'
## To create a new database, run the following command from your MySQL console:
      'mysql> CREATE DATABASE `example_database`;'
## Now you can create a new user and grant him full privileges on the database you have just created.
## The following command creates a new user named example_user, using mysql_native_password as default authentication method. We’re defining this user’s password as password, but you should replace this value with a secure password of your own choosing.
       'mysql>  CREATE USER 'example_user'@'%' IDENTIFIED WITH mysql_native_password BY 'password';'
## Now we need to give this user permission over the example_database database:
       'mysql> GRANT ALL ON example_database.* TO 'example_user'@'%';'
## This will give the example_user user full privileges over the example_database database, while preventing this user from creating or modifying other databases on your server.
## Now exit the MySQL shell with:
        'mysql> exit'
## You can test if the new user has the proper permissions by logging in to the MySQL console again, this time using the custom user credentials:
        'mysql -u example_user -p'
## Notice the -p flag in this command, which will prompt you for the password used when creating the example_user user. After logging in to the MySQL console, confirm that you have access to the example_database database:
         'mysql> SHOW DATABASES;'
## This will give you the following output:
         Output
        +--------------------+
        | Database           |
        +--------------------+
        | example_database   |
        | information_schema |
        +--------------------+
        2 rows in set (0.000 sec)
## Next, we’ll create a test table named todo_list. From the MySQL console, run the following statement:
        CREATE TABLE example_database.todo_list (
        mysql>     item_id INT AUTO_INCREMENT,
        mysql>     content VARCHAR(255),
        mysql>     PRIMARY KEY(item_id)
        mysql> );
## Insert a few rows of content in the test table. You might want to repeat the next command a few times, using different VALUES:
         'mysql> INSERT INTO example_database.todo_list (content) VALUES ("My first important item");'
## To confirm that the data was successfully saved to your table, run:
          'mysql>  SELECT * FROM example_database.todo_list;'
## You’ll see the following output:
          Output
          +---------+--------------------------+
          | item_id | content                  |
          +---------+--------------------------+
          |       1 | My first important item  |
          |       2 | My second important item |
          |       3 | My third important item  |
          |       4 | and this one more thing  |
          +---------+--------------------------+
          4 rows in set (0.000 sec)
## After confirming that you have valid data in your test table, you can exit the MySQL console:
          'mysql> exit'
## Now you can create a PHP script that will connect to MySQL and query for your content. Create a new PHP file in your custom web root directory using your preferred editor. We’ll use vi for that:
           'nano /var/www/projectLEMP/todo_list.php'
## The following PHP script connects to the MySQL database and queries for the content of the todo_list table, displays the results in a list. If there is a problem with the database connection, it will throw an exception.
## Copy this content into your todo_list.php script:
           <?php
            $user = "example_user";
            $password = "password";
            $database = "example_database";
            $table = "todo_list";

            try {
            $db = new PDO("mysql:host=localhost;dbname=$database", $user, $password);
            echo "<h2>TODO</h2><ol>";
            foreach($db->query("SELECT content FROM $table") as $row) {
            echo "<li>" . $row['content'] . "</li>";
              }
            echo "</ol>";
            } catch (PDOException $e) {
            print "Error!: " . $e->getMessage() . "<br/>";
            die();
            }
## Save and close the file when you are done editing.
## You can now access this page in your web browser by visiting the domain name or public IP address configured for your website, followed by /todo_list.php:
         'http://<Public_domain_or_IP>/todo_list.php'
## That means your PHP environment is ready to connect and interact with your MySQL server.
## Congratulations!
## In this guide, we have built a flexible foundation for serving PHP websites and applications to your visitors, using Nginx as web server and MySQL as database management system.


