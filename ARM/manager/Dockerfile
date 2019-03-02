FROM balenalib/odroid-xu4-alpine-node as build

RUN apk add --no-cache \
	bash \
	openssh \
	git \
	socat \
	python \
&& rm -rf /var/cache/apk/*

COPY keys/* /root/.ssh/

RUN chmod 600 /root/.ssh/id_rsa && eval $(ssh-agent -s) \
  && cat /root/.ssh/id_rsa | ssh-add - \
  && ssh-keyscan github.com >> ~/.ssh/known_hosts

RUN npm i -g typescript lerna

COPY docker-entrypoint.sh /usr/local/bin

ENV CACHEBUST=8

RUN apk add --no-cache --virtual .build-deps alpine-sdk python && \
    git clone git@github.com:kazazes/skyscraper-manager.git /app

RUN cd /app && \
    yarn install --pure-lockfile --network-timeout 180000

RUN cd /app && NODE_ENV=production yarn run build

EXPOSE 3000

ENTRYPOINT '/usr/local/bin/docker-entrypoint.sh'
