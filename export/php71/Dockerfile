###
## tommynovember7/php71
###

FROM php:7.1-cli

## only for java installation compatibility
# https://bugs.debian.org/cgi-bin/bugreport.cgi?bug=863199
RUN mkdir -p /usr/share/man/man1

RUN apt-get update && apt-get install -y \
    acl \
    default-jdk \
    default-jre \
    fonts-ipafont \
    g++ \
    git \
    gnupg \
    groff-base \
    libexif-dev \
    libfreetype6-dev \
    libgd-dev  \
    libgs-dev \
    libicu-dev \
    libjpeg-dev \
    libjpeg62-turbo-dev \
    libmagic-dev  \
    libmagickwand-dev \
    libmcrypt-dev \
    libmcrypt4 \
    libpng++-dev \
    libpng-dev \
    libvpx-dev  \
    make \
    mysql-client \
    openssh-client \
    openssl \
    python-pip \
    wget \
    zlib1g-dev \
    && apt-get clean

# ghostscript release versions: https://github.com/ArtifexSoftware/ghostpdl-downloads/releases
RUN cd /tmp \
    && wget https://github.com/ArtifexSoftware/ghostpdl-downloads/releases/download/gs923/ghostpdl-9.23.tar.gz \
    && tar xzvf ghostpdl-9.23.tar.gz \
    && cd ghostpdl-9.23 \
    && ./configure --disable-compile-inits --with-drivers=ALL --without-luratech --with-libiconv=gnu --prefix=/usr \
    && make \
    && make install

# imagemagick release versions: http://www.imagemagick.org/download/
RUN cd /tmp \
    && wget http://www.imagemagick.org/download/ImageMagick.tar.gz \
    && tar xzvf ImageMagick.tar.gz \
    && cd ImageMagick* \
    && ./configure --prefix=/usr --with-webp=no \
    && make \
    && make install

# nodejs
RUN curl -sL https://deb.nodesource.com/setup_6.x | bash - \
    && apt-get update \
    && apt-get install -y nodejs \
    && npm install -g npm-check-updates

# aws cli
RUN pip install --upgrade awscli

# install php modules
RUN docker-php-ext-configure intl \
    && docker-php-ext-install intl \
    && docker-php-ext-install exif \
    && docker-php-ext-install gd \
    && docker-php-ext-install mcrypt \
    && docker-php-ext-install mysqli \
    && docker-php-ext-install opcache \
    && docker-php-ext-install pcntl \
    && docker-php-ext-install pdo_mysql \
    && docker-php-ext-install zip

## see versions https://pecl.php.net/package/imagick, if failed
RUN pecl install imagick-3.4.3 \
    && docker-php-ext-enable imagick

## see versions https://xdebug.org/download.php, if failed
RUN pecl install xdebug \
    && echo "zend_extension=xdebug.so" > /usr/local/etc/php/conf.d/xdebug.ini

## add ini
RUN printf "[date]\ndate.timezone=Etc/UTC" > /usr/local/etc/php/conf.d/date.ini \
    && echo "memory_limit=512M" > /usr/local/etc/php/conf.d/memory-limit.ini

# clean up
RUN rm -rf /var/lib/apt/lists/*