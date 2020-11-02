FROM php:7.1-fpm-alpine

#  修改镜像源
RUN sed -i 's/dl-cdn.alpinelinux.org/mirrors.tuna.tsinghua.edu.cn/g' /etc/apk/repositories

# Packages
# RUN apt-get update \
#     && DEBIAN_FRONTEND=noninteractive apt-get install -y \
#         git \
#         zlib1g-dev \
#         unzip \
#         python \
#         && ( \
#             cd /tmp \
#             && mkdir librdkafka \
#             && cd librdkafka \
#             && git clone https://github.com/edenhill/librdkafka.git . \
#             && ./configure \
#             && make \
#             && make install \
#         ) \
#     && rm -r /var/lib/apt/lists/*

# PHP Extensions
# RUN docker-php-ext-install -j$(nproc) zip \
#     && pecl install rdkafka \
#     && docker-php-ext-enable rdkafka


#PHPIZE_DEPS包含了以下拓展   
# m4 autoconf dpkg-dev dpkg file perl g++ gcc binutils libatomic libgomp libc-dev musl-dev make libbz2 libmagic libstdc++ libgcc pkgconf mpc1 mpfr3 isl gmp
RUN apk add ${PHPIZE_DEPS}

# docker-php-ext-install是docker官方提供的一个安装命令，可以安装大部分php拓展，但是仍有部分拓展无法安装
#  pdo_mysql拓展
RUN docker-php-ext-install pdo_mysql

#  mysqli拓展
RUN docker-php-ext-install mysqli

# bcMatch拓展
RUN docker-php-ext-install bcmath

#  redis拓展
RUN pecl install redis-4.0.2 && docker-php-ext-enable redis

# xdebug拓展
RUN pecl install http://pecl.php.net/get/xdebug-2.9.6.tgz && docker-php-ext-enable xdebug \
	&& echo "zend_extension=$(find /usr/local/lib/php/extensions/ -name xdebug.so)" >> /usr/local/etc/php/conf.d/xdebug.ini \
	# 启用远程调试
	&& echo "xdebug.remote_enable = on" >> /usr/local/etc/php/conf.d/xdebug.ini \
	&& echo "xdebug.remote_autostart = off" >> /usr/local/etc/php/conf.d/xdebug.ini \
	# 单人开发时，即已经明确知道IDE调试器的ip和端口时，会使用到下面这两个配置
	#【IDE调试器】的所在ip和端口，用于xdebug插件连接调试器的配置。当IDE所运行的环境和PHP环境一致，可以填127.0.0.1，否则只能填IDE的运行环境ip
	&& echo "xdebug.remote_host=192.168.12.58" >> /usr/local/etc/php/conf.d/xdebug.ini \
	&& echo "xdebug.remote_port=9000" >> /usr/local/etc/php/conf.d/xdebug.ini \
	# 多人团队开发时，IDE的所在ip和端口有多个，不明确，此时不需要配置【remote_host】和【remote_port】，但是需要开启【remote_connect_back】为 on
	# 团队开发时，IDE有多个，也就意味着有多个目标ip，因此xdebug不会通过读取【remote_host】和【remote_port】的配置去连接目标IDE，而是获取请求中的
	# 【HTTP_X_FORWARDED_FOR】 和 【REMOTE_ADDR】内的ip，优先读取前者
	# xdebug 是否根据请求地址，返回调试数据（适合团队开发），即IDE所在机器ip未知/团队开发
	&& echo "xdebug.remote_connect_back=0" >> /usr/local/etc/php/conf.d/xdebug.ini \
	# 开启性能分析，分析的记录会放到 remote_log 中，需要使用 KCacheGrind工具打开
	&& echo "xdebug.profiler_enable=1" >> /usr/local/etc/php/conf.d/xdebug.ini \
	# 通知 PHP 开启调试的标识
	&& echo "xdebug.idekey=PHPSTORM" >> /usr/local/etc/php/conf.d/xdebug.ini \
	# 记录 xdebug与调试器会话 日志
	&& echo "xdebug.remote_log='/home/tmp/xdebug.log'" >> /usr/local/etc/php/conf.d/xdebug.ini

## 安装composer
RUN curl -sS https://getcomposer.org/installer \
    | php -- --install-dir=/usr/local/bin --filename=composer \
    # 国内镜像加速
    && composer config -g repo.packagist composer https://mirrors.aliyun.com/composer/

RUN apk del  ${PHPIZE_DEPS}
