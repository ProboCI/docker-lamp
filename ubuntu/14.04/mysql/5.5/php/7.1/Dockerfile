FROM proboci/ubuntu:14.04

# Install apache and enable mods.
# Curl and lynx-cur are for debugging the container.
RUN apt-get update && apt-get -y upgrade \
  && DEBIAN_FRONTEND=noninteractive apt-get -y install apache2 \
    curl \
    lynx-cur \
  && a2enmod rewrite

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
    MYSQL_LOG_DIR=/var/log/mysql \
    MYSQL_PASSWORD=strongpassword

# Install MySQL
RUN { \
    echo mysql-community-server mysql-community-server/data-dir select ''; \
    echo mysql-community-server mysql-community-server/root-pass password ''; \
    echo mysql-community-server mysql-community-server/re-root-pass password ''; \
    echo mysql-community-server mysql-community-server/remove-test-db select false; \
  } | debconf-set-selections \
  && apt-get update \
  && DEBIAN_FRONTEND=noninteractive apt-get install -y mysql-server-${MYSQL_VERSION} \
  && service mysql start \
  && mysqladmin -u root --skip-password password $MYSQL_PASSWORD \
  && service mysql stop \
  && rm -rf /var/lib/apt/lists/*

ENV PHP_VERSION 7.1
ENV WORKDIR /root

# Install PHP and enable mods.
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
    php-imagick \
    php-pear \
    php-redis \
    php-uploadprogress \
    imagemagick \
    libzip-dev \
    memcached \
    redis-server \
  && apt-get --purge autoremove language-pack-en-base -y

# Update the PHP.ini file, enable <? ?> tags and quieten logging.
RUN sed -i "s/short_open_tag = Off/short_open_tag = On/" /etc/php/$PHP_VERSION/apache2/php.ini
RUN sed -i "s/error_reporting = .*$/error_reporting = E_ERROR | E_WARNING | E_PARSE/" /etc/php/$PHP_VERSION/apache2/php.ini

# Enable php mods.
RUN phpenmod openssl

# Install drush
RUN wget -O /usr/local/bin/drush https://github.com/drush-ops/drush/releases/download/8.1.11/drush.phar \
  && chmod +x /usr/local/bin/drush \
  && mkdir /etc/drush \
  && mkdir /etc/drush/conf.d \
  && chmod -R 755 /etc/drush/conf.d

ADD drushrc.php /etc/drush/drushrc.php
ADD drush-environment.php /etc/drush/conf.d/environment.php

# Install drupal console
RUN curl https://drupalconsole.com/installer -L -o /tmp/drupal.phar \
  && mv /tmp/drupal.phar /usr/local/bin/drupal \
  && chmod +x /usr/local/bin/drupal

# Install composer
ADD install-composer /root/install-composer
RUN chmod +x /root/install-composer \
  && /root/install-composer

# Install Ruby gems
RUN apt-get update \
  && apt-get install -y ruby-dev \
  ruby-compass \
  && gem install bundler

# Install wp-cli
RUN wget -O /usr/local/bin/wp https://raw.githubusercontent.com/wp-cli/builds/gh-pages/phar/wp-cli.phar \
  && chmod 755 /usr/local/bin/wp

# Install and configure solr.
ADD zivtechaptrepo /etc/apt/sources.list.d/zivtechaptrepo.list
RUN apt-key adv --keyserver keyserver.ubuntu.com --recv-keys 02e048f5 \
  && add-apt-repository ppa:openjdk-r/ppa \
  && apt-get update \
  && apt-get install -y openjdk-8-jre \
  solr \
  && rm -rf /var/lib/apt/lists/* /tmp/*

ADD create-solr-instance /usr/local/bin/create-solr-instance
ADD remove-solr-instance /usr/local/bin/remove-solr-instance
RUN chmod +x /usr/local/bin/create-solr-instance /usr/local/bin/remove-solr-instance
