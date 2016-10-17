# nginxConfig

Author: Project4Dimensions

nginxConfig shows how to configure nginx on Ubuntu (17.10 Artful Aardvark) so as to enable PHP-FPM and change the default root folder (/var/www/html) to a development folder (e.g., /home/var/tec) owned by a user.

## Why nginxConfig?

nginx is a good front-end for services such as PHP-FPM and provides a convenient way of accessing and testing files in a development folder owned by a user.

## nginxConfig explained

Install nginx, php-fpm and nano using the Synaptic package manager or a terminal:

```
su -  # enter root password  
apt-get install nginx php-fpm nano  
logout
```

Make a copy of the default configuration file; open a terminal and paste the text below, one line at a time.

```
su -  # enter root password  
mv /etc/nginx/sites-available/default /etc/nginx/sites-available/default.dist  
cp /etc/nginx/sites-available/default.dist /etc/nginx/sites-available/default  
logout
```

The example named default shows changes made to the configuration file; the original file named default.dist shows the original unedited version.

```
su -  # enter root password  
nano /etc/nginx/sites-available/default
```

Edit line with /var/www/html and change to a user development folder (e.g., /home/var/tec):

```
root /home/var/tec;
```

To list files in folders, paste the following below the line location /

```
		autoindex on;  
		charset utf8;
```

To serve other folders, insert code after the lines location / {...}, such as the following:

```
	location /pub {  
		root /home/var;  
		index index.html index.htm;  
		autoindex on;  
		charset utf8;  
		}
```

Open `/etc/php/7.1/fpm/pool.d/www.conf` and copy the value for `listen = /run/php/php7.1-fpm.sock` (see the file, www.conf).

To enable PHP support, insert the following code after the lines location / {...}:

```
	location ~ \.php$ {  
		include snippets/fastcgi-php.conf;  
        # paste the value for listen = /run/php/php7.1-fpm.sock below  
		fastcgi_pass unix:/var/run/php/php7.1-fpm.sock;  
	}
```

Use the terminal to restart nginx, as follows:

```
su -  # enter root password  
service nginx restart  
logout
```

Paste http://localhost/ in a web browser address to access the the PHP enabled development folder of a user. If set up, other folders can be accessed by pasting something like http://localhost/pub.

Any errors such as “403 Forbidden” (see firefox-403.png) are caused by incorrect folder or file permissions. As the user of the development folder or subfolder, errors can be corrected using nautilus, the file manager. Right-click on the folder, click the tab called Permissions and check it matches nautilus-properties.png; click the button called Change Permissions for Enclosed Files… and check it matches nautilus-permissions.png.

## References

nealio82. 2013. “amazon ec2 - PHP-FPM and Nginx: 502 Bad Gateway.” https://stackoverflow.com/questions/10003978/php-fpm-and-nginx-502-bad-gateway.

nginx. 2016. “nginx news.” Accessed October 14. http://nginx.org/.

PHP-FPM. 2016. “Home - PHP-FPM” Accessed October 14. https://php-fpm.org/.

PHP Group. 2016. “PHP: FastCGI Process Manager (FPM) - Manual.” Accessed January 6. http://php.net/manual/en/install.fpm.php.

University of Chicago, The. 2010. “Chicago-Style Citation Quick Guide.” *The Chicago Manual of Style Online*. http://www.chicagomanualofstyle.org/tools_citationguide.html.
