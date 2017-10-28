# Linux server configuration 

This project involves configuring an Ubuntu 16.04 virtual machine to host a Flask web application using the Amazon Lightsail platform.

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

### Added linux users
* `catalog-user` - for running the WSGI daemon process.
* `grader` - for review and feedback purposes; sudo access provided.

### Added PostreSQL users
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


