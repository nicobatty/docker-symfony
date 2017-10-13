FROM php:7.1-cli

RUN mkdir -p /usr/local/bin \
  && curl -LsS https://symfony.com/installer -o /usr/local/bin/symfony \
  && chmod a+x /usr/local/bin/symfony

RUN mkdir /app

WORKDIR /app

RUN useradd docker

ENTRYPOINT ["symfony"]