# Alpine PHP

This PHP FPM and PHP CLI docker image based on [Alpine](https://hub.docker.com/_/alpine/). Alpine is based on [Alpine Linux](http://www.alpinelinux.org), lightweight Linux distribution based on [BusyBox](https://hub.docker.com/_/busybox/). The size of the image is very small, less than 10 MB!

## Tags

Versions and are based on PHP versions.

Here are the supported tags and respective Dockerfile links.

 * `cli`, `cli-5.6` [(Dockerfile)](https://github.com/matriphe/docker-alpine-php/blob/master/5.6/CLI/Dockerfile)
 * `fpm`, `fpm-5.6` [(Dockerfile)](https://github.com/matriphe/docker-alpine-php/blob/master/5.6/FPM/Dockerfile)
 
The `cli` tag show that the PHP is used for command line, such as simple static HTML site. And the `fpm` is disigned to be used with PHP-FPM. It is fit with [Alpine-Nginx](https://hub.docker.com/r/matriphe/alpine-nginx/) docker image.

## Getting The Image

This image is published in the Docker Hub. Simply run this command below to get it to your machine.

```Shell
docker pull matriphe/alpine-php:fpm
```

or 

```Shell
docker pull matriphe/alpine-php:cli
```

Alternatively you can clone this repository and build the image using the `docker build` command.

## Build

This image use `Asia/Jakarta` timezone by default. You can change the timezone by change the `TIMEZONE` environment on `Dockerfile` and then build.

```Shell
docker -t repository/imagename:tag Dockerfile
```

## Configuration

The site data, config, and log data is configured to be located in a Docker volume so that it is persistent and can be shared by other containers or a backup container).

There is volume defined in this image `/www` that is shared with Nginx container. Please make sure both containers can access the directory.

You can store the sites data to this directory structure:

### PHP Configuration

The config is set using environments listed below on build.

```Ini
ENV TIMEZONE Asia/Jakarta
ENV PHP_MEMORY_LIMIT 512M
ENV MAX_UPLOAD 50M
ENV PHP_MAX_FILE_UPLOAD 200
ENV PHP_MAX_POST 100M
```

## Run The Container

### Create and Run The Container

```Shell
docker run -p 9000:9000 --name phpfpm -v /home/user/public_html:/www:rw -d matriphe/alpine-php:fpm
```

If you run and want to link Nginx container, make sure you created and run this PHP-FPM the container before running this Nginx container. Make sure the `/www` volume in PHP-FPM container is mapped.

 * The first `-p` argument maps the container's port 9000 to port 9000 on the host and used for communication between Nginx and PHP-FPM.
 * `--name` argument sets the name of the container, useful when starting and stopping the container.
 * The `-v` argument maps the `/home/user/public_html` directory in the host to `/www` in the container with read/write access (rw).
 * `-d` argument runs the container as a daemon.
 
### Stopping The Container

```Shell
docker stop phpfpm
```
### Start The Container Again

```Shell
docker start phpfpm
```