FROM nginx:stable

RUN apt-get update -qq && apt-get -y install apache2-utils vim \
  && apt-get install -y ca-certificates \
  && apt-get install openssl
COPY docker/web/ssl/localhost.crt /etc/ssl/certs/localhost.crt
COPY docker/web/ssl/localhost.key /etc/ssl/private/localhost.key
RUN cd /etc/ssl/private \
  && chmod 600 localhost.* \
  && cd /etc/ssl/certs \
  && chmod 600 localhost.* \
  && update-ca-certificates --fresh

# Setting local
ENV LC_ALL C.UTF-8
# Setting timezone
ENV TZ Asia/Taipei
RUN cp /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

ENV RAILS_ROOT /var/www/todo

# Set our working directory inside the image
WORKDIR $RAILS_ROOT
# create log directory
RUN mkdir log
# copy over static assets
COPY public public/


# Copy Nginx config template
COPY docker/web/sites-enabled/todo.conf /tmp/docker.nginx


# substitute variable references in the Nginx config template for real values from the environment
# put the final config in its place
RUN envsubst '$RAILS_ROOT' < /tmp/docker.nginx > /etc/nginx/conf.d/default.conf
EXPOSE 80 443

# Use the "exec" form of CMD so Nginx shuts down gracefully on SIGTERM (i.e. `docker stop`)
CMD [ "nginx", "-g", "daemon off;" ]