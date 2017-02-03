FROM hyperiongray/starbelly-web:0.0.1

# Create a self-signed certificate.
RUN cd /tmp && \
    openssl req -x509 -newkey rsa:2048 \
                -keyout server.key -out server.crt \
                -days 365 -nodes -subj '/CN=starbelly' && \
    mv server.key server.crt /etc/nginx/tls

# Install and configure supervisor.
RUN apt-get install -y supervisor
COPY supervisor.conf /etc/supervisor/conf.d/starbelly.conf

# Override nginx.conf to proxy to pub.
COPY nginx.conf /etc/nginx/conf.d/starbelly.conf

CMD ["/usr/bin/supervisord", "-c", "/etc/supervisor/supervisord.conf"]
