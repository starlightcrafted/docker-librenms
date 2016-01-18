FROM phusion/baseimage:0.9.18

# Use baseimage-docker's init system.
CMD ["/sbin/my_init"]

## Set TERM so that nano and vi won't be weird
ENV TERM xterm

## Fix UTF-8
RUN echo "en_US.UTF-8 UTF-8" > /etc/locale.gen; \
        echo "LANG=\"en_US.UTF-8\"" > /etc/default/locale; \
        locale-gen en_US.UTF-8
RUN export LANG=en_US.UTF-8

## Create /docker
RUN mkdir /docker

## Add PHP 5.6 repo
RUN LC_ALL=en_US.UTF-8 add-apt-repository -y ppa:ondrej/php5-5.6

## Packages
RUN apt-get update && apt-get -y install nginx php5-fpm php5-mysql php5-cli php-apc php5-gd php5-snmp php-pear php5-curl snmp graphviz php5-mcrypt php5-json fping imagemagick whois mtr-tiny nmap python-mysqldb php-net-ipv4 php-net-ipv6 rrdtool git augeas-lenses augeas-tools netcat rrdcached

## Configure php5-fpm
RUN php5enmod opcache

## Download LibreNMS
RUN git clone --depth 1 https://github.com/librenms/librenms.git /opt/librenms

## Configure LibreNMS
WORKDIR /opt/librenms
RUN mkdir rrd logs

## Create LibreNMS user
RUN useradd librenms -d /opt/librenms -M -r && usermod -a -G librenms www-data
RUN chown -R librenms:librenms /opt/librenms && chown www-data:www-data logs && chmod 775 rrd

## Setup runit
RUN mkdir /etc/service/php5-fpm
ADD runit/php5-fpm /etc/service/php5-fpm/run
RUN mkdir /etc/service/nginx
ADD runit/nginx /etc/service/nginx/run
RUN mkdir /etc/service/rrdcached
ADD runit/rrdcached /etc/service/rrdcached/run

## Setup NGINX
RUN rm /etc/nginx/sites-enabled/default
ADD nginx/default /etc/nginx/sites-enabled/default
