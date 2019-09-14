FROM centos:7

## update yum
RUN yum -y update
RUN yum -y install yum-utils
RUN yum clean all

RUN yum -y install epel-release
RUN yum -y groupinstall "Development Tools"
RUN yum -y install wget git zsh curl


# ## install remi repo
# RUN wget http://rpms.famillecollet.com/enterprise/remi-release-7.rpm
# RUN rpm -Uvh remi-release-7*.rpm
# RUN yum-config-manager --enable remi-php70


# ## install php7"
# RUN \
#   yum -y install \
#     php php-common \
#     php-mbstring \
#     php-mcrypt \
#     php-devel \
#     php-xml \
#     php-mysqlnd \
#     php-pdo \
#     php-opcache --nogpgcheck \
#     php-bcmath

# install php5.6
RUN yum -y install epel-release
RUN rpm -Uvh http://rpms.famillecollet.com/enterprise/remi-release-7.rpm
RUN yum remove php-*
RUN yum -y install --enablerepo=remi, \ 
        remi-php56 \
        php  \
        php-devel  \
        php-mbstring  \
        php-pdo  \
        php-gd \
        php-opcache --nogpgcheck \
        php-bcmath \
        php-pear

## install composer
RUN curl -sS https://getcomposer.org/installer | php && \
  mv composer.phar /usr/local/bin/composer

## install xdebug
# RUN pecl install xdebug

# Install xdebug for php debugging
RUN yum install -y php-pecl-xdebug
RUN echo "xdebug.default_enable = 1" > /etc/php.d/xdebug_settings.ini \
    && echo "xdebug.remote_enable=on" >> /etc/php.d/xdebug_settings.ini \
    && echo "xdebug.remote_autostart=on" >> /etc/php.d/xdebug_settings.ini \
    && echo "xdebug.remote_connect_back = on" >> /etc/php.d/xdebug_settings.ini \
    && echo "xdebug.remote_port = 9000" >> /etc/php.d/xdebug_settings.ini

EXPOSE 80

# php setting
COPY ./php/php.ini /etc/php.d/20-jp.ini
# RUN cp ../php/xdebug.ini /etc/php.d/20-xdebug.ini

## timezone setting
RUN cp /etc/localtime /etc/localtime.org
RUN ln -sf /usr/share/zoneinfo/Asia/Tokyo /etc/localtime


## Apache install and Start
COPY docker.conf /etc/httpd/conf.d/docker.conf
RUN yum -y install httpd
RUN systemctl enable httpd


# exec.
CMD ["/sbin/init"]