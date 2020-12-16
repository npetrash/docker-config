FROM php:7.2-apache
RUN apt-get update && apt-get install -y \
        libfreetype6-dev \
        libjpeg62-turbo-dev \
        libmcrypt-dev \
        libpng-dev \
        msmtp \
        gettext \
        screen && \
        rm -r /var/lib/apt/lists/*
RUN pecl install xdebug && docker-php-ext-enable xdebug
RUN docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/
RUN docker-php-ext-install -j$(nproc) gd mysqli pdo pdo_mysql mbstring tokenizer iconv
RUN docker-php-ext-install sockets

ENV PHP_OPCACHE_VALIDATE_TIMESTAMPS="0" \
    PHP_OPCACHE_MAX_ACCELERATED_FILES="10000" \
    PHP_OPCACHE_MEMORY_CONSUMPTION="192" \
    PHP_OPCACHE_MAX_WASTED_PERCENTAGE="10"
RUN docker-php-ext-install opcache

RUN a2enmod rewrite
RUN usermod -u 1000 www-data
RUN groupmod -g 1000 www-data
COPY start.sh /usr/local/sbin/start.sh
COPY php-mail.ini /usr/local/etc/php/conf.d/mail.ini
COPY php.ini /usr/local/etc/php/conf.d/
COPY date-timezone.conf /etc/apache2/conf-enabled/date-timezone.conf
COPY encoding.conf /etc/apache2/conf-enabled/encoding.conf
COPY servername.conf.tmpl /etc/apache2/conf-enabled/servername.conf.tmpl
COPY revaliases.tmpl /etc/ssmtp
COPY ssmtp.conf.tmpl /etc/ssmtp
COPY cron_events.sh /usr/local/sbin/cron_events.sh
 RUN sed -i 's/\r$//' /usr/local/sbin/start.sh  && \  
        chmod +x /usr/local/sbin/start.sh
CMD ["/usr/local/sbin/start.sh"]

