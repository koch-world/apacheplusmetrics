FROM ubuntu:18.04
RUN apt-get update && \
    apt-get install --no-install-recommends -y apt-utils perl build-essential cpanminus apache2 nano supervisor && \
    apt-get clean && rm -rf /var/lib/apt/lists/*
RUN cpanm JSON JSON::XS FindBin && \
    rm -rf /root/.cpanm

# supervisor installation &&
# create directory for child images to store configuration in
RUN mkdir -p /var/log/supervisor && \
    mkdir -p /etc/supervisor/conf.d

WORKDIR /code
COPY httpd-exporter /code
RUN mkdir -p /etc/httpd-exporter
COPY exporter.conf /etc/httpd-exporter
COPY index.html /var/www/html
COPY apache2.conf /etc/apache2/apache2.conf

# supervisor base configuration
ADD supervisor.conf /etc/supervisor.conf

# default command
CMD ["supervisord", "-c", "/etc/supervisor.conf"]
