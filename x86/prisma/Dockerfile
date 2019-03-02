FROM ubuntu

ENV LANG=C.UTF-8

RUN apt-get update && \
    apt-get install --no-install-recommends -y \
    build-essential \
    git \
    openjdk-8-jdk \   
    gnupg \
    && rm -rf /var/lib/apt/lists/*

RUN echo "deb https://dl.bintray.com/sbt/debian /" | tee -a /etc/apt/sources.list.d/sbt.list \
    && apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 2EE0EA64E40A89B84B2DF73499E82A75642AC823

RUN apt-get update && \
    apt-get install --no-install-recommends -y \
    sbt \
    && rm -rf /var/lib/apt/lists/*

RUN git clone https://github.com/prisma/prisma \
    && cd /prisma \
    && git checkout $(git describe --tags $(git rev-list --tags --max-count=1))

WORKDIR /prisma/server

RUN COMMIT_SHA=$(git rev-parse HEAD) CLUSTER_VERSION=1.8 sbt docker
