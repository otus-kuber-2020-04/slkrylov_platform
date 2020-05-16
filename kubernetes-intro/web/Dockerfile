FROM nginx:1.17.10-alpine
LABEL maintainer="S.L Krylov"
LABEL info="Homework kubernetes-intro/web"

ENV NGINX_PORT=8000
ENV USER=otus
ENV UID=1001
ENV GID=1001

COPY app /app
COPY nginx/conf.d /etc/nginx/conf.d

RUN \
	addgroup --gid $GID $USER &&\ 
	adduser --uid $UID -D -G $USER -s /sbin/nologin $USER &&\
	sed -i "s|^user.*;|user ${USER};|g" /etc/nginx/nginx.conf &&\
	sed -i "s|listen.*;|listen ${NGINX_PORT};|g" /etc/nginx/conf.d/default.conf &&\
	chmod 1777 /run &&\
	chown $USER:$USER /var/cache/nginx && \
	chown $USER:$USER /var/log/nginx 

USER "$USER"
EXPOSE "$NGINX_PORT"
 
