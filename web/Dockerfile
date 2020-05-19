FROM nginx:1.11
ENV LANG=C.UTF-8
ENV LC_ALL=C.UTF-8
ENV PUB_CACHE=/var/cache/pub

# Install OS packages.
RUN apt-get update && \
    apt-get install -y apt-transport-https curl apache2-utils unzip && \
    rm -fr /var/cache/apt/archives/*

# Install Dart
RUN curl 'https://storage.googleapis.com/dart-archive/channels/stable/release/1.19.0/sdk/dartsdk-linux-x64-release.zip' -o /opt/dartsdk-linux-x64-release.zip
RUN cd /opt && unzip dartsdk-linux-x64-release.zip
ENV PATH=$PATH:/opt/dart-sdk/bin

# Configure nginx.
RUN rm /etc/nginx/conf.d/default.conf && \
    mkdir /etc/nginx/tls
COPY nginx.conf /etc/nginx/conf.d/starbelly.conf

# Get Starbelly source code.
COPY dependencies/starbelly-web-client /starbelly-web-client

# Build the web client code (and work around file permissions bug when running
# pub as root).
RUN mkdir -p $PUB_CACHE && \
    cd /starbelly-web-client && \
    pub get && \
    chown -R root:root "$PUB_CACHE" && \
    find "$PUB_CACHE" -type f -exec chmod 644 {} \; && \
    find "$PUB_CACHE" -type d -exec chmod 755 {} \; && \
    pub build

ENV PS1='starbelly-web:$PWD# '

# Container init script.
COPY container-init.sh /container-init.sh
RUN chmod +x /container-init.sh
CMD /container-init.sh
