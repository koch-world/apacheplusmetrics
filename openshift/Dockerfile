FROM ubuntu:18.04
LABEL maintainer="martin@koch-world.de"
LABEL description="Dockerfile for apache 2.4 w/ a metrics endpoint for prometheus"
LABEL version="1.0"

RUN apt-get update && \
    apt-get install --no-install-recommends -y apt-utils perl build-essential cpanminus apache2 nano supervisor && \
    apt-get clean && rm -rf /var/lib/apt/lists/*
RUN cpanm JSON JSON::XS FindBin && \
    rm -rf /root/.cpanm

WORKDIR /code
COPY httpd-exporter /code/httpd-exporter
RUN mkdir -p /var/log/supervisor && \
    mkdir -p /etc/supervisor/conf.d && \
    mkdir -p /etc/httpd-exporter && \
    mkdir -p /var/run/apache2 && chmod 777 -R /var/run/apache2 &&\
    chmod -R 777 /code && \
    mkdir -p /var/log/apache2 && chmod 777 -R /var/log/apache2 &&\
    mkdir -p /var/lock/apache2 && chmod 777 -R /var/lock/apache2 &&\
    mkdir -p /etc/apache2/sites-enabled && chmod 777 -R /etc/apache2/sites-enabled &&\
    mkdir -p /var/www/html && chmod 777 -R /var/www/html
COPY exporter.conf /etc/httpd-exporter
COPY apache2.conf ports.conf /etc/apache2/

# supervisor base configuration
ADD supervisor.conf /etc/supervisor.conf

EXPOSE 8000
USER 1001

# example page - metrics file will be stored as /var/www/html/metrics 
COPY index.html /var/www/html

# default command
CMD ["supervisord", "-c", "/etc/supervisor.conf"]
