FROM php:8.2-fpm
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer
RUN apt-get update \
    && apt-get install -y \
        mc git \
        libzip-dev \
        zip \
        strace \
        htop \
    && docker-php-ext-install pdo pdo_mysql mysqli zip \
    && docker-php-ext-enable mysqli zip
COPY docker.conf /usr/local/etc/php-fpm.d/docker.conf
COPY php.ini /usr/local/etc/php/php.ini
ENV TERM xterm-256color
RUN echo PS1="'"'\[\e]0;\u@\h: \w\a\]${debian_chroot:+($debian_chroot)}\ \033[01;32m\]\u@\h\[\033[00m\]:\[\033[01;34m\]\w\[\033[00m\]\$ '"'" >> ~/.bashrc
