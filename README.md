# AWS_WordPress
Installing Wordpress in AWS Ec2 instance and running in private domain

Step1:
Create your EC2 Instance and open instance in terminal via SSH

Step2:
update linux
sudo apt-get update && sudo apt-get upgrade -y

Step3:
Install required applications
sudo apt install apache2 php libapache2-mod-php php-mysql mysql-server -y

Step4:
Creating Database and user {edit you credentials between <> }
sudo mysql -u root
>ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password by '<Your Password>';
>CREATE USER '<Your User ID>'@localhost IDENTIFIED BY '<Your Password>';
>CREATE DATABASE <Your Database Name>;
>GRANT ALL PRIVILEGES ON <Your Database Name>.* TO '<Your User ID>'@localhost;
>exit

Step5:
Download Wordpress
cd /var/www/
sudo wget https://wordpress.org/latest.tar.gz
sudo tar -xvf latest.tar.gz
sudo rm -r latest.tar.gz

Step6:
setting up the permissions
sudo chown -R www-data:www-data wordpress
sudo find wordpress/ -type d -exec chmod 755 {} \;
sudo find wordpress/ -type f -exec chmod 644 {} \;

Step7:
edit wordpress config file
sudo nano /etc/apache2/sites-available/000-default.conf 

Edit configfile
DocumentRoot /var/www/wordpress
if you have external domain add this in the document
ServerName mshaik.tech
ServerAlias www.mshaik.tech

Step8:
cd /var/www/wordpress
sudo nano wp-config.php
add this in the end of the file
define("FS_METHOD","direct");

Step9:
Restarting server
sudo systemctl restart apache2
