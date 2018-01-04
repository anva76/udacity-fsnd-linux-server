# Linux Server Configuration 

This project involves setting up an Ubuntu 16.04 virtual machine to host a Flask web application utilizing the Amazon Lightsail platform.

## Main parameters
* IP Address: 18.221.83.150
* URL: http://ec2-18-221-83-150.us-east-2.compute.amazonaws.com/
* SSH port: 2200

## Installed software
* Flask
* SQLAlchemy
* git
* PostreSQL
* Apache HTTP Server
* libapache2-mod-wsgi-py3
* python3-pip
* python-dev

## Configuration summary
### SSH Access
The SSH port has been changed from 22 to 2200. For remote access via the `grader` user, please use the following command:
`ssh -i <key-file> -p 2200 grader@18.221.83.150`.

### Created Linux Users
* `catalog-user` - for running the WSGI daemon process.
* `grader` - for review and feedback purposes; sudo access provided.

### Created PostreSQL Users
* `catalog` - for database access to perform CRUD operations.

### UFW Firwall configuration
```
Status: active
To                         Action      From
--                         ------      ----
80/tcp                     ALLOW       Anywhere                  
2200                       ALLOW       Anywhere                  
123                        ALLOW       Anywhere  
```

### Apache Server Configuration
```
<VirtualHost *:80>

        ServerAdmin webmaster@localhost
        DocumentRoot /home/catalog-user/catalog
        ServerName localhost

        WSGIDaemonProcess yourapplication user=catalog-user group=catalog-user threads=5
        WSGIScriptAlias / /home/catalog-user/catalog/catalog.wsgi

        <Directory /home/catalog-user/catalog/>
                WSGIProcessGroup yourapplication
                WSGIApplicationGroup %{GLOBAL}
                Order allow,deny
                Allow from all
                Require all granted
        </Directory>

        ErrorLog ${APACHE_LOG_DIR}/error.log
        CustomLog ${APACHE_LOG_DIR}/access.log combined

</VirtualHost>

```

### Directory Listings
#### /home/catalog-user/catalog
```
ubuntu@ip-172-26-14-219:/home/catalog-user/catalog$ ls -la
total 64
drwxr-xr-x 5 catalog-user catalog-user  4096 Oct 28 00:23 .
drwxr-xr-x 6 catalog-user catalog-user  4096 Oct 27 22:55 ..
-rw-rw-r-- 1 catalog-user catalog-user 18782 Oct 27 22:37 catalog_app.py
-rw-rw-r-- 1 catalog-user catalog-user  1877 Oct 27 22:37 catalog.sql
-rw-rw-r-- 1 catalog-user catalog-user   125 Oct 27 22:51 catalog.wsgi
-rw-rw-r-- 1 catalog-user catalog-user   571 Oct 27 22:37 client_secrets.json
-rw-rw-r-- 1 catalog-user catalog-user  2010 Oct 27 22:37 db_setup.py
-rw-rw-r-- 1 catalog-user catalog-user  2905 Oct 27 22:37 google_auth.py
drwxr-xr-x 2 catalog-user catalog-user  4096 Oct 27 22:56 __pycache__
-rw-rw-r-- 1 catalog-user catalog-user    19 Oct 27 22:37 README.md
drwxr-xr-x 6 catalog-user catalog-user  4096 Oct 27 22:37 static
drwxr-xr-x 2 catalog-user catalog-user  4096 Oct 27 22:37 templates
```
#### /home/catalog-user/catalog/static
```
ubuntu@ip-172-26-14-219:/home/catalog-user/catalog/static$ ls -la
total 24
drwxr-xr-x 6 catalog-user catalog-user 4096 Oct 27 22:37 .
drwxr-xr-x 5 catalog-user catalog-user 4096 Oct 28 00:23 ..
drwxr-xr-x 2 catalog-user catalog-user 4096 Oct 27 22:37 css
drwxr-xr-x 2 catalog-user catalog-user 4096 Oct 27 22:37 images
drwxr-xr-x 2 catalog-user catalog-user 4096 Oct 27 22:37 js
drwxr-xr-x 2 catalog-user catalog-user 4096 Oct 28 00:20 uploads
```

## Third Party Resources
* http://flask.pocoo.org/docs/0.12/deploying/mod_wsgi/
* https://www.postgresql.org/docs/9.5/static/index.html
* https://aws.amazon.com/ru/premiumsupport/knowledge-center/new-user-accounts-linux-instance/
* http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-key-pairs.html#retrieving-the-public-key

