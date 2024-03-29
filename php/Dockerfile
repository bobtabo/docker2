FROM php:7.2-fpm

# ライブラリのインストール
RUN apt-get update
RUN apt-get install -y \
      sudo \
      libpq-dev \
      git \
      zip \
      unzip \
      vim
RUN docker-php-ext-install \
      mbstring \
      pdo \
      bcmath \
      pdo_mysql

# Xdebugのインストール
RUN pecl install xdebug-2.9.8
RUN docker-php-ext-enable xdebug

# Composerのインストール
COPY --from=composer:1 /usr/bin/composer /usr/bin/composer

# ユーザーを作成
ARG DOCKER_UID=1000
ARG DOCKER_USER=docker
ARG DOCKER_PASSWORD=docker
      RUN useradd -m --uid ${DOCKER_UID} --groups sudo ${DOCKER_USER} \
      && echo ${DOCKER_USER}:${DOCKER_PASSWORD} | chpasswd \
      && echo "docker    ALL=(ALL)       ALL" >> /etc/sudoers

# ソケットを置く場所
RUN mkdir -p /var/run/php-fpm \
      && chown docker:docker /var/run/php-fpm

# 設定の置換
COPY php.ini /usr/local/etc/php/php.ini
COPY xdebug.ini /usr/local/etc/php/conf.d/xdebug.ini
COPY www.conf /usr/local/etc/php-fpm.d/zzz-www.conf

WORKDIR /home/docker
