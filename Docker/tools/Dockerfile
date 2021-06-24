FROM php:7.3.28-fpm
ARG TIMEZONE

RUN useradd -ms /bin/bash tools
RUN chown tools /home/tools

RUN apt-get update && apt-get install -y \
    openssl \
    git \
    libz-dev \
    unzip

RUN docker-php-ext-install zip

# Install Composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

RUN curl -sL https://deb.nodesource.com/setup_8.x | bash -
RUN apt-get install -y nodejs
RUN curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | apt-key add -
RUN echo "deb https://dl.yarnpkg.com/debian/ stable main" | tee /etc/apt/sources.list.d/yarn.list
RUN apt-get update
RUN apt-get install -y yarn

# Type docker-php-ext-install to see available extensions
RUN docker-php-ext-install pdo pdo_mysql

RUN chmod a+w /tmp

WORKDIR /tmp