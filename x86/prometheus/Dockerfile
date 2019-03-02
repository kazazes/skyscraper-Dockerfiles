FROM balenalib/intel-nuc-debian:latest

# versions
ENV NODE_EXPORTER_VERSION 0.12.0

# arch
ENV DIST_ARCH linux-armv7

# downloading utils
RUN apt-get update && apt-get install -y wget build-essential libc6-dev

WORKDIR /etc

# get node exporter
RUN wget https://github.com/prometheus/node_exporter/releases/download/$NODE_EXPORTER_VERSION/node_exporter-$NODE_EXPORTER_VERSION.$DIST_ARCH.tar.gz
RUN tar xvfz node_exporter-$NODE_EXPORTER_VERSION.$DIST_ARCH.tar.gz && rm node_exporter-$NODE_EXPORTER_VERSION.$DIST_ARCH.tar.gz

WORKDIR /

COPY start.sh ./

# run when container lands on device
CMD ["bash", "start.sh"]
