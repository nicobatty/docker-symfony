FROM php:7.2-fpm

RUN curl -sS -o /tmp/icu.tar.gz -L http://download.icu-project.org/files/icu4c/63.1/icu4c-63_1-src.tgz \
  && tar -zxf /tmp/icu.tar.gz -C /tmp \
  && cd /tmp/icu/source \
  && ./configure --prefix=/usr/local \
  && make \
  && make install \
  && rm -rf /tmp/icu*

RUN apt-get update \
  && apt-get install -y \
    sudo \
    zlib1g-dev \
    zip \
    git \
  && rm -rf /var/lib/apt/lists/*

RUN docker-php-ext-configure intl --with-icu-dir=/usr/local \
  && docker-php-ext-install intl opcache pcntl zip

# User Docker for permission managment
RUN useradd docker \
  && mkdir -p /home/docker/.composer \
  && usermod --append --groups sudo docker \
  && chown -R docker:docker /home/docker \
  && echo 'ALL ALL = (ALL) NOPASSWD: ALL' >> /etc/sudoers \
  && echo 'docker:secret' | chpasswd


# Setup the Composer installer

ENV COMPOSER_VERSION 1.8.5
RUN curl --silent --fail --location --retry 3 --output /tmp/installer.php --url https://raw.githubusercontent.com/composer/getcomposer.org/cb19f2aa3aeaa2006c0cd69a7ef011eb31463067/web/installer \
  && php -r " \
    \$signature = '48e3236262b34d30969dca3c37281b3b4bbe3221bda826ac6a9a62d6444cdb0dcd0615698a5cbe587c3f0fe57a54d8f5'; \
    \$hash = hash('sha384', file_get_contents('/tmp/installer.php')); \
    if (!hash_equals(\$signature, \$hash)) { \
      unlink('/tmp/installer.php'); \
      echo 'Integrity check failed, installer is either corrupt or worse.' . PHP_EOL; \
      exit(1); \
    }" \
  && php /tmp/installer.php --no-ansi --install-dir=/usr/bin --filename=composer --version=${COMPOSER_VERSION} \
  && composer --ansi --version --no-interaction \
  && rm -f /tmp/installer.php

ENV APP_ROOT /var/www/html
WORKDIR /var/www/html

COPY php.ini /usr/local/etc/php/php.ini
COPY docker-php-entrypoint /usr/local/bin/docker-php-entrypoint
