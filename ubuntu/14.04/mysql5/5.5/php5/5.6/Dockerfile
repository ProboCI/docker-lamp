FROM proboci/ubuntu:14.04


RUN apt-get update
RUN apt-get -y upgrade

# Install apache, PHP, and supplimentary programs. curl and lynx-cur are for debugging the container.
RUN DEBIAN_FRONTEND=noninteractive apt-get -y install apache2 libapache2-mod-php5 php5-mysql php5-gd php-pear php-apc php5-curl php5-ldap curl lynx-cur

# Enable apache mods.
RUN php5enmod openssl
RUN a2enmod php5
RUN a2enmod rewrite

# Update the PHP.ini file, enable <? ?> tags and quieten logging.
RUN sed -i "s/short_open_tag = Off/short_open_tag = On/" /etc/php5/apache2/php.ini
RUN sed -i "s/error_reporting = .*$/error_reporting = E_ERROR | E_WARNING | E_PARSE/" /etc/php5/apache2/php.ini

# Manually set up the apache environment variables
ENV APACHE_RUN_USER www-data
ENV APACHE_RUN_GROUP www-data
ENV APACHE_LOG_DIR /var/log/apache2
ENV APACHE_LOCK_DIR /var/lock/apache2
ENV APACHE_PID_FILE /var/run/apache2.pid

EXPOSE 80

# Update the default apache site with the config we created.
ADD apache-config.conf /etc/apache2/sites-enabled/000-default.conf

# By default, simply start apache.
CMD /usr/sbin/apache2ctl -D FOREGROUND

ENV MYSQL_USER=mysql \
    MYSQL_VERSION=5.5 \
    MYSQL_DATA_DIR=/var/lib/mysql \
    MYSQL_RUN_DIR=/run/mysqld \
    MYSQL_LOG_DIR=/var/log/mysql

RUN apt-get update \
 && DEBIAN_FRONTEND=noninteractive apt-get install -y mysql-server-${MYSQL_VERSION} \
 && rm -rf /var/lib/apt/lists/*

EXPOSE 3306/tcp

ENV PHP_VERSION 5.6
ENV WORKDIR /root

RUN apt-get update && DEBIAN_FRONTEND=noninteractive apt-get install -y \
	language-pack-en-base \
  && LC_ALL=en_US.UTF-8 add-apt-repository ppa:ondrej/php \
  && apt-get update && DEBIAN_FRONTEND=noninteractive apt-get install -y \
  	php$PHP_VERSION \
  	php$PHP_VERSION-cli \
  	php$PHP_VERSION-common \
  	php$PHP_VERSION-curl \
  	php$PHP_VERSION-dev \
  	php$PHP_VERSION-gd \
  	php$PHP_VERSION-json \
  	php$PHP_VERSION-ldap \
  	php$PHP_VERSION-mbstring \
  	php$PHP_VERSION-mcrypt \
  	php$PHP_VERSION-mysql \
  	php$PHP_VERSION-readline \
  	php$PHP_VERSION-xml \
  	php$PHP_VERSION-zip \
  	libapache2-mod-php$PHP_VERSION \
  	php5-imagick \
  	php-pear \
  	php-redis \
  	php-uploadprogress \
  	imagemagick \
  	libzip-dev \
  	memcached \
  	redis-server \
  && apt-get --purge autoremove language-pack-en-base -y \
  && rm -rf /var/lib/apt/lists/* /tmp/*
