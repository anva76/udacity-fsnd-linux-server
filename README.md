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
* `catalog` - for database access to perforum CRUD operations.

### Firwall configuration
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

## Third Party Resources
* http://flask.pocoo.org/docs/0.12/deploying/mod_wsgi/
* https://www.postgresql.org/docs/9.5/static/index.html
* https://aws.amazon.com/ru/premiumsupport/knowledge-center/new-user-accounts-linux-instance/
* http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-key-pairs.html#retrieving-the-public-key

