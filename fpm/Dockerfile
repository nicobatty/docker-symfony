FROM php:7.1-fpm

RUN curl -sS -o /tmp/icu.tar.gz -L http://download.icu-project.org/files/icu4c/59.1/icu4c-59_1-src.tgz \
  && tar -zxf /tmp/icu.tar.gz -C /tmp \
  && cd /tmp/icu/source \
  && ./configure --prefix=/usr/local \
  && make \
  && make install \
  && rm -rf /tmp/icu*

ENV PHP_CPPFLAGS="$PHP_CPPFLAGS -std=c++11"

RUN docker-php-ext-configure intl --with-icu-dir=/usr/local \
  && docker-php-ext-install intl opcache

COPY ./php.ini /usr/local/etc/php/php.ini
