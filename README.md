# BOOKSTORE

## Ruby version: 2.7.1

## Rails version: 6.0.3.3

## Gems used: MySql2 and bootstrap

## Setting up MySql:

    * first run (installs mysql, apache2 and phpmyadmin): 
    
            sudo apt-get install lamp-server^

    * alternative for installing mysql:

            https://linuxize.com/post/how-to-install-mysql-on-ubuntu-18-04/

    * After installation run the following commands for the setup of mysql root password:
        
            sudo mysql
                ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'my_new_pass';
                FLUSH PRIVILEGES;
            mysql -u root -p my_new_pass

    * In mysql terminal (that you access via previous command), run the following commands:

            CREATE DATABASE databasename_development;
            CREATE DATABASE databasename_test;
            
            CREATE USER 'newuser'@'localhost' IDENTIFIED BY 'password';
            GRANT ALL PRIVILEGES ON databasename_development. * TO 'newuser'@'localhost';
            GRANT ALL PRIVILEGES ON databasename_test. * TO 'newuser'@'localhost';

            SHOW DATABASES;
            SELECT HOST, USER FROM mysql.user;

            FLUSH PRIVILEGES;
            EXIT;

    * Also see for additional info on user control: 
    
            https://www.digitalocean.com/community/tutorials/how-to-create-a-new-user-and-grant-permissions-in-mysql

## Also install:

        sudo apt-get install libmyclient-dev
        sudo apt-get install phpmyadmin

## Setting up apache2:

    * Follow the link, to make sure the apache2 is installed and configured: 
    
            https://www.digitalocean.com/community/tutorials/how-to-install-the-apache-web-server-on-ubuntu-20-04
    
    * After installation: 
    
    * if sudo ufw status returns inactive run: 
        
            sudo ufw enable

    * next to see your ip address:
    
            hostname -I | awk '{print $1}' to see your ip address
        
    * run in your browser to see if apache2 is working correctly  
        
            http://your_ip_adress 

    * run to see your host name and mark it down (it's the one that is not marked as localhost)
            
            sudo nano /etc/hosts 

    * run to configure phpmyadmin:
            
            sudo nano /etc/apache2/apache2.conf

    * Add the following two lines to that file:
            
            Include /etc/phpmyadmin/apache.conf

            ServerName your_host_name (the one that you marked down previously)

    * run in your browser to see if everything is working:

            localhost/phpmyadmin

## Installing gems:

    * run:
        
            'gem install mysql2'    

## Editing gemfile:

    * change:

            gem 'mysql2' instead of gem 'sqlite3'
            gem 'therubyracer', platforms: :ruby

## Editing database.yml:

    * change at default:

            'adapter: sqlite3' to 'adapter: mysql2'
    
    * change at development: 

            'adapter: sqlite3' to 'adapter: mysql2'
            'database: db/development.sqlite3' to 'database: databasename(the one that you created in mysql)_development'

        * add:
        
                username: newuser
                password: my_new_password
                (login creditentials for user that has access to the databases that you want, and also the one that you previously created)

    * change at test:

            exact same thing as before, but instead of databasename_development use databasename_test

## Nodejs and yarn:

    * If for some reason yarn doesn't want to be install, it's probably the outdated nodejs version.
      To fix that run the following commands:

            cd ~
            curl -sL https://deb.nodesource.com/setup_10.x -o nodesource_setup.sh
            sudo bash nodesource_setup.sh
            sudo apt install nodejs
            nodejs -v

    * At the end run: 

            yarn install --check-files

## phpmyadmin warning fix:

    * Just run this command:

            sudo sed -i "s/|\s*\((count(\$analyzed_sql_results\['select_expr'\]\)/| (\1)/g" /usr/share/phpmyadmin/libraries/sql.lib.php