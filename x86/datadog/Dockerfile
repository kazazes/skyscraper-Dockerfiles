FROM balenalib/intel-nuc-golang:latest-build AS build

RUN install_packages sysstat python python-dev python-pip libpython-dev python-setuptools git

COPY requirements.txt requirements.txt
RUN pip install -r requirements.txt

ENV GOPATH=/usr/app
ENV PATH=$PATH:/usr/app/bin

RUN git clone --branch 6.8.3 --depth 1 https://github.com/DataDog/datadog-agent.git /usr/app/src/github.com/DataDog/datadog-agent

WORKDIR /usr/app/src/github.com/DataDog/datadog-agent

RUN export PATH=$PATH:$GOPATH/bin && \
  cd /usr/app/src/github.com/DataDog/datadog-agent && \
  invoke deps && \
  invoke agent.build --build-exclude=snmp,systemd

FROM balenalib/intel-nuc-debian

WORKDIR /usr/app

COPY --from=build ./usr/app/src/github.com/DataDog/datadog-agent/bin/ /usr/app/build/
COPY files /usr/app/files
RUN chmod +x files/start.sh

RUN ln -s /usr/app/build/agent/dist/ /etc/datadog-agent

RUN mkdir /etc/datadog-agent/conf.d/disk.d
RUN mkdir /etc/datadog-agent/conf.d/network.d

RUN ln -s /usr/app/files/disk.yaml /etc/datadog-agent/conf.d/disk.d/conf.yaml.default
RUN ln -s /usr/app/files/network.yaml /etc/datadog-agent/conf.d/network.d/conf.yaml.default

RUN install_packages sysstat python libpython2.7

CMD ./files/start.sh
