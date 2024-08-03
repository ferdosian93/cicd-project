FROM harbor.tasn.ir/library/node:16-10 as builder
ENV NODE_ENV production
WORKDIR /home/pwa

# copy required files installing deps & building project
COPY package.json .
COPY tsconfig.json .
COPY yarn.lock .
COPY public .
COPY src .

RUN yarn install --production=false && yarn run build

FROM harbor.tasn.ir/library/docker-base-images/nginx:latest-v2

WORKDIR /var/www
ENV GRAYLOG_SERVER="172.25.25.51:12201"

COPY ./ci/nginx/config/ /etc/nginx/

RUN rm /etc/nginx/conf.d/default.conf && \
	mkdir /etc/nginx/sites-enabled/

COPY ./ci/nginx/vhosts/ /etc/nginx/sites-enabled/

COPY --from=builder /home/pwa/sample .

COPY ./ci/docker/entrypoint.sh /usr/local/bin/entrypoint.sh

RUN  chmod +x /usr/local/bin/entrypoint.sh

ENTRYPOINT ["/usr/local/bin/entrypoint.sh"]