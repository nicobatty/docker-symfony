# Docker Symfony Base Image

Docker FPM / NGINX base images for Symfony that satisfy all requirements and recommendations.

If you want to try out the images, you can test out the associated `docker-compose.yml` file here

Feel free to update the file to fit your needs.

## Stack

* PHP-FPM 7.3
* NGINX 1.17

The FPM image does not contain any PDO extensions since it is not a Symfony requirement. You can create a new image from this one if you want to add it.

## Tools

* Symfony command
* Composer 1.6.4

## Symfony Install

In order to create a new Symfony project, run this command:

```
docker run --rm -it --user docker -v ~/.gitconfig:/home/docker/.gitconfig -v $PWD:/var/www/html nbatty/symfony-fpm symfony new <my_project_name>
```

### Starting Server

In order to start the server, copy the `docker-compose.yml` file to the newly created project and run:

```
docker-compose up -d
```

You should then be able to access your project from this URL in your browser (replace localhost with the guest IP if running in a VM):

```
http://localhost:8080/
```
