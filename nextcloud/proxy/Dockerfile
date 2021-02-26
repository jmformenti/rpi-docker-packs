FROM nginx:1.19.3

# Install wget and install/updates certificates
RUN apt-get update \
 && apt-get install -y -q --no-install-recommends \
    ca-certificates \
    wget \
    git \
 && apt-get clean \
 && rm -r /var/lib/apt/lists/*

# Configure Nginx and apply fix for very long server names
RUN echo "daemon off;" >> /etc/nginx/nginx.conf \
 && sed -i 's/worker_processes  1/worker_processes  auto/' /etc/nginx/nginx.conf

# Install Forego
RUN wget --quiet https://bin.equinox.io/c/ekMN3bCZFUn/forego-stable-linux-arm64.tgz && \
	tar xvf forego-stable-linux-arm64.tgz -C /usr/local/bin && \
	chmod u+x /usr/local/bin/forego

ENV DOCKER_GEN_VERSION 0.7.4

ADD docker-gen-linux-arm64-0.7.4.tar.gz /usr/local/bin

#RUN wget https://github.com/jwilder/docker-gen/releases/download/$DOCKER_GEN_VERSION/docker-gen-linux-amd64-$DOCKER_GEN_VERSION.tar.gz \
# && tar -C /usr/local/bin -xvzf docker-gen-linux-amd64-$DOCKER_GEN_VERSION.tar.gz \
# && rm /docker-gen-linux-amd64-$DOCKER_GEN_VERSION.tar.gz

ENV NGINX_PROXY_VERSION "0.8.0"

RUN git clone --branch ${NGINX_PROXY_VERSION} https://github.com/jwilder/nginx-proxy.git /app

COPY network_internal.conf /etc/nginx/

COPY timeout.conf /etc/nginx/conf.d/

WORKDIR /app/

ENV DOCKER_HOST unix:///tmp/docker.sock

VOLUME ["/etc/nginx/certs", "/etc/nginx/dhparam"]

ENTRYPOINT ["/app/docker-entrypoint.sh"]
CMD ["forego", "start", "-r"]