FROM ubuntu:22.04

RUN apt-get update
WORKDIR /buildroot
COPY prerequisites.sh scripts/prerequisites.sh
RUN scripts/prerequisites.sh && \
    apt-get install -y python3-distutils && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

RUN echo '#! /bin/bash\n. oe-init-build-env 2>/dev/null\n"$@"' > /usr/local/bin/docker-entrypoint.sh && \
    chmod +x /usr/local/bin/docker-entrypoint.sh 
ENTRYPOINT ["/usr/local/bin/docker-entrypoint.sh"]
