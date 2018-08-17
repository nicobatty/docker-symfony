# Docker Symfony Base Image

Docker FPM / NGINX base images for Symfony that satisfy all requirements and recommendations.

If you want to try out the images, you can test out the associated `docker-compose.yml` file here.

Feel free to update the file to fit your needs.

## Stack

* PHP-FPM 7.1
* NGINX 1.15

The FPM image does not contain any PDO extensions since it is not a Symfony requirement. You can create a new image from this one if you want to add it.

## Tools

* Symfony installer command
* Composer 1.6.4

## Demo

The best way to demonstrate how these images work is to use it on the official Symfony demo.

First, install the demo:

```
mkdir app
docker-compose run --rm installer demo
```

Then to run the demo:

```
docker-compose up -d web
```

You should then be able to access the working demo from the URL in your browser (replace localhost with the guest IP if running in a VM):

```
http://localhost:8080/
```
