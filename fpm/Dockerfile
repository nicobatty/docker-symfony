FROM php:7.1-fpm

RUN curl -sS -o /tmp/icu.tar.gz -L http://download.icu-project.org/files/icu4c/62.1/icu4c-62_1-src.tgz \
  && tar -zxf /tmp/icu.tar.gz -C /tmp \
  && cd /tmp/icu/source \
  && ./configure --prefix=/usr/local \
  && make \
  && make install \
  && rm -rf /tmp/icu*

RUN apt-get update \
  && apt-get install -y sudo \
  && rm -rf /var/lib/apt/lists/*

RUN docker-php-ext-configure intl --with-icu-dir=/usr/local \
  && docker-php-ext-install intl opcache pcntl

# User Docker for permission managment
RUN useradd docker \
  && mkdir -p /home/docker/.composer \
  && usermod --append --groups sudo docker \
  && chown -R docker:docker /home/docker \
  && echo 'ALL ALL = (ALL) NOPASSWD: ALL' >> /etc/sudoers \
  && echo 'docker:secret' | chpasswd

ENV APP_ROOT /var/www/html
WORKDIR /var/www/html

COPY php.ini /usr/local/etc/php/php.ini
COPY docker-php-entrypoint /usr/local/bin/docker-php-entrypoint
