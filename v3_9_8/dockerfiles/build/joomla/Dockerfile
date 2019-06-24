FROM joomla:3.9.8-fpm

LABEL maintainer="info@optimum-web.com"


# no tty for container
ENV DEBIAN_FRONTEND noninteractive

RUN apt-get update && apt-get install -y supervisor

RUN groupadd -r -g 2000 joomla; useradd -r -u 2000 -g 2000 -m -c "app account" -d /home/joomla -s /bin/bash joomla

WORKDIR /var/www/html

RUN chown -R joomla. /run/php && \
    chown -R joomla. /var/log/supervisor

# Copy wp-config
COPY config/www.conf /etc/php/7.2/fpm/pool.d
COPY config/supervisord.conf /etc/supervisor/conf.d/joomla.conf
COPY config/php-fpm.conf /etc/php/7.2/fpm/php-fpm.conf

# ----------------------------------------------------------
# PHP-fpm Configgi
# ----------------------------------------------------------
RUN sed -i -e "s/;cgi.fix_pathinfo\s*=\s*1/cgi.fix_pathinfo = 0/g; s/expose_php\s*=\s*On/expose_php = Off/g" \
/etc/php/7.2/fpm/php.ini
RUN sed -i -e "s/expose_php\s*=\s*On/expose_php = Off/g" /etc/php/7.2/fpm/php.ini
RUN sed -i -e "s/upload_max_filesize\s*=\s*2M/upload_max_filesize = 100M/g; s/post_max_size\s*=\s*8M/post_max_size = 100M/g" \
/etc/php/7.2/fpm/php.ini
RUN sed -i -e "s/;daemonize\s*=\s*yes/daemonize = no/g" /etc/php/7.2/fpm/php-fpm.conf
RUN sed -i -e"s/^;opcache.enable\s*=\s*0/opcache.enable = 1/; s/^;opcache.max_accelerated_files\s*=\s*2000/opcache.max_accelerated_files = 4000/" /etc/php/7.2/fpm/php.ini

VOLUME ["/var/www/html", "/var/log"]
EXPOSE 9000
