FROM ubuntu:latest
ENV DEBIAN_FRONTEND=noninteractive

RUN apt-get update \
 && apt-get install -y \
   git \
   sudo \
   ruby-full \
   build-essential \
   zlib1g-dev \
 && rm -rf /var/lib/apt/lists/* \
 && gem install bundler --no-document


# Create a user for VS Code
RUN echo "ubuntu ALL=(ALL) NOPASSWD:ALL" > /etc/sudoers.d/ubuntu \
 && chmod 0440 /etc/sudoers.d/ubuntu
