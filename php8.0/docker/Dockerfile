FROM shenron/nginx:1.20.1

MAINTAINER Thomas CASTELLY <thomas@tcy.io>

ENV DEBIAN_FRONTEND noninteractive

ENV LD_LIBRARY_PATH /usr/local/instantclient/

# Force to install PHP7
RUN apt-get update && apt-get -y install apt-transport-https curl lsb-release ca-certificates wget &&\
  wget -O /etc/apt/trusted.gpg.d/php.gpg https://packages.sury.org/php/apt.gpg &&\
  echo "deb https://packages.sury.org/php/ $(lsb_release -sc) main" > /etc/apt/sources.list.d/php.list

RUN apt-get update && apt-get install -y \
  # For memcached
  pkg-config \
  php8.0-xml \
  php8.0-dev \
  php8.0-fpm \
  php8.0-curl \
  php-pear \
  unzip \
  libaio-dev \
  libmemcached-dev \
  libzip-dev \
  unzip

# Copy link config between PHP and Nginx
COPY /conf/nginx/ /etc/nginx/conf.d/

# Copy base PHP config
COPY /conf/php/ /etc/php/8.0/fpm/

# install oracle client
COPY /conf/oracle/ /tmp/

RUN unzip /tmp/instantclient-basiclite-linux.x64-19.3.0.0.0dbru.zip -d /usr/local/ && \
  unzip /tmp/instantclient-sdk-linux.x64-19.3.0.0.0dbru.zip -d /usr/local/ && \
  rm /tmp/instantclient-basiclite-linux.x64-19.3.0.0.0dbru.zip /tmp/instantclient-sdk-linux.x64-19.3.0.0.0dbru.zip &&\
  ln -s /usr/local/instantclient_19_3 /usr/local/instantclient && \
  echo 'instantclient,/usr/local/instantclient' | pecl install oci8 && \
  echo "extension=oci8.so" >> /etc/php/8.0/fpm/php.ini


# Install other extensions
RUN pecl install\
  xdebug-3.0.0\
  memcached\
  zip

# Install PHPUnit
RUN wget https://phar.phpunit.de/phpunit.phar &&\
  chmod +x phpunit.phar && \
  mv phpunit.phar /usr/local/bin/phpunit

COPY /conf/entrypoint.sh /
COPY /conf/entrypoint-nginx-conf.sh /

ENTRYPOINT ["/entrypoint.sh"]

WORKDIR /var/www

EXPOSE 80 443
