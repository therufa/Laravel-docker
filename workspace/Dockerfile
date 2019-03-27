FROM phusion/baseimage:latest

# Switch to non-interactive mode
ENV DEBIAN_FRONTEND noninteractive

# Switch to root
USER root

# Create the workspace user
ARG PUID=1000
ENV PUID ${PUID}
ARG PGID=1000
ENV PGID ${PGID}

# Add the PHP 7 ppa
RUN apt-get install -y software-properties-common && \
	add-apt-repository -y ppa:ondrej/php

# Run apt-get update and clean up at end, create user and group
RUN set -xe; \
    apt-get update -yqq && apt-get upgrade -yqq && \
    groupadd -g ${PGID} wsuser && \
    useradd -u ${PUID} -g wsuser -m wsuser -G docker_env && \
    usermod -p "*" wsuser -s /bin/bash && \
	apt-get install -yqq --allow-downgrades --allow-remove-essential \
        --allow-change-held-packages \
        apt-utils \
        curl \
        git \
        libcurl4-openssl-dev \
        libedit-dev \
        libsqlite3-dev \
        libssl-dev \
        libxml2-dev \
        libzip-dev \
        mysql-client \
        nano \
        php7.3-bcmath \
        php7.3-cli \
        php7.3-common \
        php7.3-curl \
        php7.3-dev \
        php7.3-gd \
        php7.3-intl \
        php7.3-json \
        php7.3-mbstring \
        php7.3-memcached \
        php7.3-mysql \
        php7.3-pgsql \
        php7.3-sqlite \
        php7.3-sqlite3 \
        php7.3-xml \
        php7.3-zip \
        pkg-config \
        postgresql-client \
        sqlite3 \
        unzip \
        vim \
        xz-utils \
        zip && \
	apt-get clean

# Python
RUN apt-get install -yqq python python-pip python-dev build-essential  \
	&& python -m pip install --upgrade pip  \
	&& python -m pip install --upgrade virtualenv

# Composer
RUN curl -fsSL http://getcomposer.org/installer | php && \
    echo "export PATH=${PATH}:/var/www/vendor/bin" >> ~/.bashrc && \
    echo "export PATH=${PATH}:/var/www/vendor/bin" >> /home/wsuser/.bashrc && \
	mv composer.phar /usr/local/bin/composer && \
	chmod 777 /usr/local/bin/composer


# NVM
USER wsuser
ARG WS_INSTALL_NODEJS=false
ENV WS_INSTALL_NODEJS ${WS_INSTALL_NODEJS}
ENV NVM_DIR /home/wsuser/.nvm
RUN if [ ${WS_INSTALL_NODEJS} = true ]; then \
	mkdir -p /home/wsuser/.nvm && \
	curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.34.0/install.sh | bash && \
    . $NVM_DIR/nvm.sh && \
    nvm install node && \
    nvm use node && \
    npm install -g gulp && \
    npm install -g yarn && \
    npm install -g bower \
;fi

# Copy NVM settings to root's .bashrc
USER root
RUN if [ ${WS_INSTALL_NODEJS} = true ]; then \
	echo "" >> ~/.bashrc && \
    echo 'export NVM_DIR="/home/wsuser/.nvm"' >> ~/.bashrc && \
    echo '[ -s "$NVM_DIR/nvm.sh" ] && . "$NVM_DIR/nvm.sh"  # This loads nvm' >> ~/.bashrc \
;fi

# Add nodejs PATH
ENV PATH $PATH:/home/wsuser/.node-bin

# Clean up
RUN apt-get clean && \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* && \
    rm /var/log/lastlog /var/log/faillog

# Update the bashrc file
RUN . ~/.bashrc

# Set the workdir to the wsuser's home
WORKDIR /var/www

# Force the container to keep running...
ENTRYPOINT tail -f /dev/null