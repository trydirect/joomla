FROM joomla:3.9.8-php7.2-fpm

LABEL maintainer="info@optimum-web.com"


# no tty for container
ENV DEBIAN_FRONTEND noninteractive

RUN apt-get update && apt-get install -y supervisor procps curl

RUN groupadd -r -g 2000 joomla; useradd -r -u 2000 -g 2000 -m -c "app account" -d /home/joomla -s /bin/bash joomla

WORKDIR /var/www/html

RUN mkdir -p /run/php && \
    chown -R joomla. /run/php && \
    chown -R joomla. /var/log/supervisor

# Copy wp-config
COPY config/www.conf /usr/local/etc/php-fpm.d
COPY config/supervisord.conf /etc/supervisor/conf.d/joomla.conf
COPY config/php-fpm.conf /usr/local/etc/php-fpm.conf

# ----------------------------------------------------------
# PHP-fpm Configgi
# ----------------------------------------------------------
RUN sed -i -e "s/;daemonize\s*=\s*yes/daemonize = no/g" /usr/local/etc/php-fpm.conf

VOLUME ["/var/www/html", "/var/log"]
EXPOSE 9000
