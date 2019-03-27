FROM webdevops/apache:ubuntu-16.04

ARG DOCUMENT_ROOT=/var/www/
ENV WEB_DOCUMENT_ROOT=${DOCUMENT_ROOT}

ARG PHP_CONTAINER_NAME=php-fpm
ARG PHP_CONTAINER_PORT=9000
ENV WEB_PHP_SOCKET=${PHP_CONTAINER_NAME}:${PHP_CONTAINER_PORT}

EXPOSE 80 443

WORKDIR /var/www/

COPY custom.apache.conf /etc/apache2/sites-enabled/custom.apache.conf

ENTRYPOINT ["/opt/docker/bin/entrypoint.sh"]

CMD ["supervisord"]
