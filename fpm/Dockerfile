FROM php:8.1-fpm

RUN curl -sS -o /tmp/icu.tar.gz -L https://github.com/unicode-org/icu/releases/download/release-69-1/icu4c-69_1-src.tgz \
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
    libzip-dev \
    zip \
    git \
    wget \
  && rm -rf /var/lib/apt/lists/*

RUN docker-php-ext-configure intl \
  && docker-php-ext-install intl opcache pcntl zip

# User Docker for permission managment
RUN useradd docker \
  && mkdir -p /home/docker/.composer \
  && usermod --append --groups sudo docker \
  && chown -R docker:docker /home/docker \
  && echo 'ALL ALL = (ALL) NOPASSWD: ALL' >> /etc/sudoers \
  && echo 'docker:secret' | chpasswd

COPY --from=composer:2 /usr/bin/composer /usr/bin/composer

ENV XDEBUG_ENABLED 0

# Add Xdebug
RUN pecl install -o -f xdebug \
  && rm -rf /tmp/pear \
  && { \
      echo "zend_extension=/usr/local/lib/php/extensions/no-debug-non-zts-20210902/xdebug.so"; \
      echo "xdebug.start_with_request=yes"; \
  } > /usr/local/etc/php/conf.d/ext-xdebug.ini.disabled

ENV APP_ROOT /var/www/html
WORKDIR /var/www/html

COPY php.ini /usr/local/etc/php/php.ini
COPY docker-php-entrypoint /usr/local/bin/docker-php-entrypoint

RUN wget https://get.symfony.com/cli/installer -O - | bash \
  && mv /root/.symfony/bin/symfony /usr/local/bin/symfony

