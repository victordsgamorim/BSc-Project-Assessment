# BSc-Project-Report

## Introduction to Oracle VM VirtualBox and Ubuntu 16.04 installation

Virtual machines are extremely powerful as they allow you to share hardware resources from one machine to run multiple operating 
systems and software on virtual machines. Such as creating systems for studies, creating production environments, testing and software 
developments, etc. There are still many other benefits we can extract from virtualization, but there are drawbacks in using virtual 
machines when virtualization is done at the software level. The main disadvantage is the performance difference between virtual 
machines and real machines. In other words, the program creates a kind of Server or O.S image so we can work more safely.
Ubuntu is an operating system and is perfect for laptops, desktops and servers. Whether for use at home, college or work, Ubuntu 
contains all the tools from word processor and e-mail reader to web servers and programming tools, is one of the preferred operating 
systems for developers. Due to its stability, easy installation of development environments and the power that the terminal provides. 
Ubuntu allows you to develop any kind of platform inside it, if in specific cases you need Windows environments. For creating the image 
of Ubuntu in Oracle VM Virtual Machine you need to have the file downloaded from the site itself. I was able to go into New and create 
a name, type (Linux) and version (Ubuntu 32-bits / 64-bits) of the image. An amount of RAM is required to be allocated in the virtual 
machine and a space is required for all processed information to be saved. 

# Installing LAMP stack

After installing the Ubuntu 16.04, the principal part of having a web server in the computer is the LAMP stack. Short for Linux, 
Apache, MySQL and PHP, This set of programs put on the PC with (L) Linux an Apache web server (A), the web-oriented programming 
language (PHP) and MySQL database (M). 
In order to update the operating system I used “sudo apt-get upgrade " so that you can install the following programs.
The first step will be to install the Apache HTTP server, which is a software (or program) that runs in the background under an 
appropriate operating system and supports multi-tasking, and provides services to other applications that connect to it, such as client 
web Browsers. Installing Apache on Linux does require a bit of programming skills, so I used the following command for installation.

	$ sudo apt-get install apache2 apache2-utils

I modified the Apache settings in order for the web server to start in system boot, then another command was used to get the service 
started.

	$ sudo systemctl enable apache2
	$ sudo systemctl start apache2
	
Once activated, I modified the Network ports of the image, going in Advanced / Port Fowarding, changing the Host and Guest Port, 
both to 80 and applying an "Apache" name. So that I could have the certainty that everything was working well and entered the link, 
http: // localhost: 80 /, the page will be able to load if it is working.

Then the MySQL installation, where it is an open source relational database management system used in most free applications to manage 
its databases. The service uses the Structure Query Language (SQL) language, which is the most popularlanguage for entering, 
accessing, and managing content stored in a database. Using the following command, I was able to install the program together 
with the server. 

	$ sudo apt-get install mysql-client mysql-server
	
A password was required for the reason to my web page's data access get protected, but there are cases where we need to 
enforce security for sites with more data importance. So I used the following command.

	$ sudo mysql_secure_installation
	
For the creation of a database and a user, I entered the MySQL server itself using a command and applied the previous password 
that was requested when the installation was performed.

	$ mysql –u root –p
	CREATE DATABASE wordpressDATABASE;
	CREATE USER 'wordpressUSER'@'localhost' IDENTIFIED BY 'SENHA';

After creating both, ensure that the database acted in cooperation with the user.

	GRANT ALL PRIVILEGES ON wordpressDATABASE.* TO 'wordpressUSER'@'localhost';
	FLUSH PRIVILEGES;
	
The FLUSH statement has several variant forms that clear or reload various internal caches, flush tables, or acquire locks, after 
flusing.

	EXIT;
	
Back to the Linux terminal, I applied the following command in order to install PHP, which will be responsible for processing 
information on your page in WordPress

	$ sudo apt-get install php7.0 php7.0-mysql libapache2-mod-php7.0 php7.0-cli php7.0-cgi php7.0-gd 

When setting up our LAMP stack, we only need a minimal set of extensions, so PHP can communicate with MySQL. WordPress and many 
of its plugins require additional PHP extensions.

	$ sudo apt-get install php-curl php-gd php-mbstring php-mcrypt php-xml php-xmlrpc

Each WordPress plugin has its own set of requirements. Some may require additional PHP packages to be installed.  
To test if PHP is working in collaboration with the web server, I needed to create a 'infor.php' file inse /var/www/html/

	$ sudo vi /var/www/html/info.php

Inside the file 'info.php' I pressed “i” so that I could insert data inside the file.

	<?php
	phpinfo();
	?>
	
ESC and :wq so I could go out and save it, so that a test would be done in order to know if PHP is working accordingly, I logged into 
the web browser and typed, http: //localhost/info.php. A page will open confirming the process.

## WordPress Installation

WordPress is one of the most popular content management and blog platforms in the world. It allows us  to easily create and manage
a website and provides a practically endless number of plugins and themes for extra functionality.  Downloading wordpress, I used 
the following command.

	$ wget –c https://wordpress/latest.tar.gz
	
In my case, I had to install apt-get wget because there was not on my computer, so I used.

	$ sudo apt-get install wget 
	
Done that, I continued with my process of downloading and installing WordPress, after all the upload the file needs to be extracted.

	$ tar –xzvf latest.tar.gz
	
All extracted content must be redirected to another folder, so I used the following command so that everything can be transferred.

	$ sudo rsync –av wordpress/* /var/www/html/
	
Next, I set the correct permissions on the website directory, that is give ownership of the WordPress files to the web server as 
follows.

	$ sudo chown -R www-data:www-data /var/www/html/
	
Then changed permissions to allow anyone to read and execute the file but only me will be able to write. 

	$ sudo chmod -R 755 /var/www/html/

I accessed the / var / www / html / directory to rename the wordpress file.

	cd /var/www/html/
	$ sudo mv wp-config-sample.php wp-config.php

Next, I had to modify some database connection settings at the beginning of the file. I needed to adjust the 
database name, the database user, and the associated password that was configured within MySQL. I saved and closed the file
when finished.

	define('DB_NAME', 'wordpressDATABSE');
	/** MySQL database username */
	define('DB_USER', 'wordpressUSER');
	/** MySQL database password */
	define('DB_PASSWORD', 'SENHA');

Done that I must restart the Apache and MySQL server for that process to successfully follow.

	$ sudo systemctl restart apache2.service
	$ sudo systemctl restart mysql.service

Entering the domain http: //localhost/wp-config.php I can confirm that it is working, then I created a new user for the page and a 
new password. 

Wordpress page successfully created.

## Possibles Erros

During my research to create a page in wordpress, on a different computer I could see that I had not been able to do it in 
my own university, without having knowledge of what the problem was, I went behind and then I discovered that 
the '.htacess' file was disabled.

WordPress and many WordPress plugins use these files extensively for adjustments in directories to change the behavior of 
the web server.I opened the main Apache configuration file to make the first change.

	$ sudo nano /etc/apache2/apache2.conf

To allow the .htaccess files, I had to set the AllowOverride directive inside the Directory block pointing to 
the root folder (document root). At the bottom of the file, I added the following block, then saved it and closed it.

	<Directory /var/www/html/>
    	 AllowOverride All
	</Directory>

Next, I enabled mod_rewrite so that I could use the permalink functionality of WordPress.

	$ sudo a2enmod rewrite

Before we implemented the changes I made, I checked that I did not make any kind of mistake while I enabled the file.

	$ sudo apache2ctl configtest

The confirmation that the syntax was right would be.

	AH00558: apache2: Could not reliably determine the server's fully qualified domain name, using 127.0.1.1. Set the 
	'ServerName' directive globally to suppress this messageSyntax OK
	
Again I restarted the Apache and MySQL server and I could see that the WordPress site was working. I was able to log in to 
the page and then configure, then I noticed that the index was not running, again I went to the Linux server and went to 
the correct directory and used the list command so I could see what files were in the folder.

	$ cd /var/www/html/
	$ ls
	
There was a file called index.html, which meant that whenever I entered the main wordpress page, it was redirected to the
Apache home page, so I deleted it. I updated the page with Ctrl + 5 so that the database was also updated.

	$ sudo rm index.html

And then, my wordpress page was ready.
 
