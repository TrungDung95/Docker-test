FROM php:5.6-cli

LABEL author.name="Dungnt" \
      author.email="dungnt.fit@gmail.com"
ENV TZ=Asia/Ho_Chi_Minh
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

ENV APP_PATH /my_app
WORKDIR $APP_PATH

RUN docker-php-ext-install mysqli

#install gearman extension
RUN apt-get update
#RUN apt-get install -y php5.6-dev
#RUN 5
RUN apt-get install -y libgearman-dev
RUN curl -L -o /tmp/gearman-1.1.2.tgz https://pecl.php.net/get/gearman-1.1.2.tgz \
    && tar xfz /tmp/gearman-1.1.2.tgz \
    && rm -r /tmp/gearman-1.1.2.tgz \
    && cd gearman-1.1.2 && phpize && ./configure && make && make install
# RUN cd /tmp
# RUN curl -O https://pecl.php.net/get/gearman-1.1.2.tgz
# RUN tar -xzvf gearman-1.1.2.tgz
# RUN cd gearman-1.1.2 && phpize && ./configure && make && make install
RUN mkdir -p /usr/lib/php/5.6/extensions
RUN mv gearman-1.1.2/modules/gearman.so /usr/lib/php/5.6/extensions/gearman.so
RUN cp /usr/local/etc/php/php.ini-production /usr/local/etc/php/php.ini
RUN echo "extension=gearman.so" >> /usr/local/etc/php/php.ini

# RUN cd /tmp
# RUN https://github.com/phpredis/phpredis/archive/4.3.0.tar.gz
# RUN tar -xzvf phpredis-4.3.0.tar.gz
# RUN cd phpredis && phpize && ./configure && make && make install
# RUN echo "extension=redis.so" >> /usr/local/etc/php/php.ini

RUN curl -L -o /tmp/redis.tar.gz https://github.com/phpredis/phpredis/archive/4.3.0.tar.gz \
    && tar xfz /tmp/redis.tar.gz \
    && rm -r /tmp/redis.tar.gz \
    && cd phpredis-4.3.0 && phpize && ./configure && make && make install
RUN mv phpredis-4.3.0/modules/redis.so /usr/lib/php/5.6/extensions/redis.so
RUN echo "extension=redis.so" >> /usr/local/etc/php/php.ini

RUN docker-php-ext-install pdo_mysql
RUN echo "extension=pdo_mysql.so" >> /usr/local/etc/php/php.ini

COPY .. $APP_PATH

COPY entrypoint.sh /usr/bin/
RUN chmod +x /usr/bin/entrypoint.sh
ENTRYPOINT ["entrypoint.sh"]
EXPOSE 80

CMD ["php", "server", "-b", "0.0.0.0"]