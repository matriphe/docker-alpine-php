# Alpine PHP

[![Build Status](https://travis-ci.org/matriphe/docker-alpine-php.svg?branch=master)](https://travis-ci.org/matriphe/docker-alpine-php)

This PHP FPM and PHP CLI docker image based on [Alpine](https://hub.docker.com/_/alpine/). Alpine is based on [Alpine Linux](http://www.alpinelinux.org), lightweight Linux distribution based on [BusyBox](https://hub.docker.com/_/busybox/). The size of the image is very small, less than 50 MB!

## Tags

Versions and tags are based on PHP versions.

Here are the supported tags and respective Dockerfile links.

 * `cli`, `cli-5.6` [(Dockerfile)](https://github.com/matriphe/docker-alpine-php/blob/master/5.6/CLI/Dockerfile)
 * `fpm`, `fpm-5.6` [(Dockerfile)](https://github.com/matriphe/docker-alpine-php/blob/master/5.6/FPM/Dockerfile)
 * `fpm7`, `fpm-7` [(Dockerfile)](https://github.com/matriphe/docker-alpine-php/blob/master/7.0/FPM/Dockerfile)
 
The `cli` tag show that the PHP is used for command line and the `fpm` is designed to be used with PHP-FPM. It is fit with [Alpine-Nginx](https://hub.docker.com/r/matriphe/alpine-nginx/) docker image.

### PHP 7.0

PHP 7.0 is still using [**edge/testing** repository for PHP7 packages](https://pkgs.alpinelinux.org/packages?name=php7*&branch=&repo=testing&arch=&maintainer=). PHP 7.0 CLI is not available yet, since there's no `php7-cli` package in the repository.

## Getting The Image

This image is published in the [Docker Hub](https://hub.docker.com/r/matriphe/alpine-php/). Simply run this command below to get it to your machine.

```Shell
docker pull matriphe/alpine-php:fpm
```

Or if you want to use `cli` image. 

```Shell
docker pull matriphe/alpine-php:cli
```

Alternatively you can clone this repository and build the image using the `docker build` command.

## Build

This image use `Asia/Jakarta` timezone by default. You can change the timezone by change the `TIMEZONE` environment on `Dockerfile` and then build.

```Shell
docker build -t repository/imagename:tag .
```

## Configuration

The site data, config, and log data is configured to be located in a Docker volume so that it is persistent and can be shared by other containers or a backup container).

There is volume defined in this image `/www` that is shared with Nginx container. Please make sure both containers can access the directory. You can store the sites data to this directory.

### PHP Configuration

The config is set using environments listed below on build.

```Ini
ENV TIMEZONE Asia/Jakarta
ENV PHP_MEMORY_LIMIT 512M
ENV MAX_UPLOAD 50M
ENV PHP_MAX_FILE_UPLOAD 200
ENV PHP_MAX_POST 100M
```

Change it in `Dockerfile` and you can rebuild your image.

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

## PHP CLI

To ease the job, make alias for PHP-CLI. Add this line to `~/.bashrc` so we can call it as regular `php` command.

```Shell
alias php='docker run --rm --name=php-cli -v $(pwd):/www matriphe/alpine-php:cli php'
```