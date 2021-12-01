# Step 1: Migrate the Content 

#### Make a copy of original wordpress content 
```tar -zcvf pluto.tar pluto ```

#### Make a copy of original mysql db 
```cd /var/lib/mysql ```

```mysqldump -p plutwp > pluto.sql ```

#### Start a Lightsail wp instance and connect through ssh 
From Connection section to retrieve the instance's public IP address
Download the default private key LightsailDefaultKey-az.cer 

```ssh bitnami@public-ip -i LightsailDefaultKey-az.cer ```

#### Retrieve the application password
```cat bitnami_application_password```

#### Access Wordpress site through Internet 
To login the wp instance, http://publicip/wp-admin

```username: user

password: same as the application password ```

#### Upload original wp files to Lightsail instance	
```scp pluto* publicip:```

#### Create database and mysql user on Lightsail Instance 	
Login mysql in Lightsail instance, password is same as the application password

```mysql -p ```

Database name, mysql user and its password can be found from the original wp site's wp-config.php file 

create database plutowp;

```CREATE USER 'username'@'localhost' IDENTIFIED BY 'password';

GRANT ALL PRIVILEGES ON plutowp.* to 'username'@'localhost';

FLUSH PRIVILEGES;```

#### Migrate the wordpress database to the Lightsail instance 	
```mysql -p plutowp < plutowp.sql ```

####Modify the new wp-config.php	
unpack the original wp content

```tar -zxvf pluto.tar . ```

from its wp-config.php copy the following lines and paste to /opt/bitnami/apps/wordpress/htdocs/wp-config.php and remove the existing correspondent lines  

```*/** The name of the database for WordPress */
define('DB_NAME', 'plutowp');

/** MySQL database username */
define('DB_USER', 'username');

/** MySQL database password */
define('DB_PASSWORD', 'password'); *```

#### Reload the Lightsail Wordpress site to update username and password	
reload http://public-ip/wp-admin, to agree to update username and password, re-login 

#### Copy the upload directory contents 	
cp -a pluto/wp-content/uploads/*  /opt/bitnami/apps/wordpress/htdocs/wp-content/uploads/

#### Install themes 	
Get the name of the theme from pluto/wp-content/theme

install the theme from Lightsail wp instance web console

#### Install the plugins 	
Get the name of the plugins from pluto/wp-content/plugs

Install one by one 

# Step 2 reserve DNS with Neteng

# Step 2 reserve DNS with Neteng

