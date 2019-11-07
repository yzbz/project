## Introduction
This is a Dockerfile to build a container image for nginx and php-fpm, with the ability to pull website code from git. The container also has the ability to update templated files with vaiables passed to docker in order to update your settings.

### Git repository
The source files for this project can be found here: [https://github.com/yzbz/project](https://github.com/yzbz/project)

If you have any improvements please submit a pull request.
### Docker hub repository
The Docker hub build can be found here: [registry.cn-beijing.aliyuncs.com/zhangpeng_test/nginx-php/](registry.cn-beijing.aliyuncs.com/zhangpeng_test/nginx-php/)
## Versions
- Nginx: **1.10.1**
- PHP: **5.6.21**
- Alpine Version: **3.4**

## Building from source
To build from source you need to clone the git repo and run docker build:
```
https://github.com/yzbz/project.git
docker build -t nginx-php-fpm:latest .
```

## Pulling from Docker Hub
Pull the image from docker hub rather than downloading the git repo. This prevents you having to build the image on every docker host:
```
docker pull registry.cn-beijing.aliyuncs.com/zhangpeng_test/nginx-php
```

## Running
To simply run the container:
```
sudo docker run -d registry.cn-beijing.aliyuncs.com/zhangpeng_test/nginx-php
```

You can then browse to ```http://<DOCKER_HOST>:8080``` to view the default install files. To find your ```DOCKER_HOST``` use the ```docker inspect``` to get the IP address.
### Volumes
If you want to link to your web site directory on the docker host to the container run:
```
sudo docker run -d -v /your_code_directory:/var/www/html registry.cn-beijing.aliyuncs.com/zhangpeng_test/nginx-php
```
### Enabling SSL or Special Nginx Configs
You can either map a local folder containing your configs  to /etc/nginx or we recommend editing the files within __conf__ directory that are in the git repo, and then rebuilding the base image.


### Templating
**NOTE: You now need to enable templates see below**
This container will automatically configure your web application if you template your code.

### Using environment variables
For example if you are using a MySQL server, and you have a config.php file where you need to set the MySQL details include $$_MYSQL_HOST_$$ style template tags.

Example create config.php.tmpl::
```
<?php
database_host = ${MYSQL_HOST};
...
?>
```
To set the variables simply pass them in as environmental variables on the docker command line.

Example:
```
sudo docker run -d -e 'MYSQL_HOST=host.x.y.z' nginx-php-fpm
```

This will expose the following variables that can be used to template your code.
```
database_host=host.x.y.z
```
## Logging and Errors

### Logging
All logs should now print out in stdout/stderr and are available via the docker logs command:
```
docker logs <CONTAINER_NAME>
```

### Displaying Errors
If you want to display PHP errors on screen (in the browser) for debugging purposes use this feature:
```
-e ERRORS=1
```
