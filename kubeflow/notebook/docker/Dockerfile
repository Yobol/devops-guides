FROM debian:12.0

ENV DEBIAN_FRONTEND=noninteractive

USER root

# https://mirrors.ustc.edu.cn/help/debian.html
RUN sed -i 's/deb.debian.org/mirrors.ustc.edu.cn/g' /etc/apt/sources.list.d/debian.sources

RUN apt-get update && \
    apt-get install -y --no-install-recommends \
        ca-certificates \
        curl \
        gnupg \
        lsb-release
    
RUN curl -fsSL https://download.docker.com/linux/debian/gpg | gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
    
RUN echo \
      "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/debian \
      $(lsb_release -cs) stable" | tee /etc/apt/sources.list.d/docker.list > /dev/null

RUN apt update && \
    apt install -y --no-install-recommends \
        docker-ce-cli \
        docker-compose-plugin \
        && \
    apt clean && rm -rf /tmp/* && rm -rf /var/tmp/*

# docker build -f Dockerfile -t demo:dind .
# docker run --rm -it --name dind -v /var/run/docker.sock:/var/run/docker.sock demo:dind docker ps
