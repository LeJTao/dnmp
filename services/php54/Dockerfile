ARG PHP_VERSION
FROM ${PHP_VERSION}

ARG TZ
ARG PHP_EXTENSIONS
ARG CONTAINER_PACKAGE_URL

RUN echo "Acquire" >> /etc/apt/apt.conf \
    && echo "{" >> /etc/apt/apt.conf \
    && echo '  http::User-Agent "Mozilla/5.0 (Windows NT 5.1; rv:25.0) ' >> /etc/apt/apt.conf \
    && echo 'Gecko/20100101 Firefox/25.0";' >> /etc/apt/apt.conf \
    && echo '};'  >> /etc/apt/apt.conf \
	&& echo "" > /etc/apt/sources.list \
	&& echo "deb http://mirrors.cloud.tencent.com/debian jessie main contrib non-free" >> /etc/apt/sources.list \
	&& echo "deb http://mirrors.cloud.tencent.com/debian jessie-updates main contrib non-free" >> /etc/apt/sources.list \
	&& echo "deb-src http://mirrors.cloud.tencent.com/debian jessie main contrib non-free" >> /etc/apt/sources.list \
	&& echo "deb-src http://mirrors.cloud.tencent.com/debian jessie-updates main contrib non-free" >> /etc/apt/sources.list \
    && apt-get update

COPY ./extensions /tmp/extensions
WORKDIR /tmp/extensions
RUN chmod +x install.sh \
    && sh install.sh \
    && rm -rf /tmp/extensions

# Install composer and change it's cache home
RUN curl -o /usr/bin/composer https://mirrors.aliyun.com/composer/composer.phar \
    && chmod +x /usr/bin/composer
ENV COMPOSER_HOME=/tmp/composer


# php image's www-data user uid & gid are 82, change them to 1000 (primary user)
RUN usermod -u 1000 www-data && groupmod -g 1000 www-data


WORKDIR /www
