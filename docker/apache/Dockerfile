FROM centos:6

# Install & Add repo
RUN rpm -Uvh https://dl.fedoraproject.org/pub/epel/epel-release-latest-6.noarch.rpm
RUN rpm -Uvh http://rpms.famillecollet.com/enterprise/remi-release-6.rpm

RUN yum -y update
RUN yum clean all

# Install PHP5.6 & Apache
RUN yum --enablerepo=remi-php56 install php php-devel php-gd php-xml php-mbstring php-opcache php-mcrypt php-mysqlnd -y

# Install Xdebug
RUN yum --enablerepo=epel,remi-php56 install php-pecl-xdebug -y

# Configure php.ini
RUN sed -ri 's/;date.timezone =/date.timezone = Asia\/Tokyo/' /etc/php.ini

RUN sed -i '$a[xdebug]' /etc/php.ini
RUN sed -i '$azend_extension="/usr/lib64/php/modules/xdebug.so"' /etc/php.ini
RUN sed -i '$axdebug.idekey = "PHPSTORM"' /etc/php.ini
RUN sed -i '$axdebug.max_nesting_level=1000' /etc/php.ini
RUN sed -i '$axdebug.remote_autostart=1' /etc/php.ini
RUN sed -i '$axdebug.remote_host=10.254.254.254' /etc/php.ini
RUN sed -i '$axdebug.remote_enable=1' /etc/php.ini
RUN sed -i '$axdebug.remote_handler=dbgp' /etc/php.ini
RUN sed -i '$axdebug.remote_port=9000' /etc/php.ini
RUN sed -i '$axdebug.profiler_enable=1' /etc/php.ini
RUN sed -i '$axdebug.profiler_output_dir=/tmp' /etc/php.ini
RUN sed -i '$axdebug.remote_log=/tmp/xdebug.log' /etc/php.ini

# Clear yum cache
RUN yum clean all

# Install PostFix
RUN yum install postfix -y

#Install Supervisor and config
RUN yum install -y supervisor
RUN touch /etc/supervisord.conf
RUN echo '[supervisord]'  >> /etc/supervisord.conf
RUN echo 'nodaemon=true'  >> /etc/supervisord.conf
RUN echo '[program:httpd]' >> /etc/supervisord.conf
RUN echo 'command=/usr/sbin/httpd -D FOREGROUND'   >> /etc/supervisord.conf
RUN echo '[program:postfix]' >> /etc/supervisord.conf
RUN echo 'command=/usr/sbin/postfix -c /etc/postfix start'   >> /etc/supervisord.conf

# Clear yum cache
RUN yum clean all

# for web
EXPOSE 80

CMD ["/usr/bin/supervisord", "-n"]
