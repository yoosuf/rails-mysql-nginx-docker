# Base image:
# FROM nginx
FROM nginx:1.15.8

RUN apt-get update -qq && apt-get -y install apache2-utils vim

ENV RAILS_ROOT /var/www/app_name


# Setting local
ENV LC_ALL C.UTF-8
# Setting timezone
ENV TZ Asia/Taipei
RUN cp /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone


# Set our working directory inside the image
WORKDIR $RAILS_ROOT
# create log directory
RUN mkdir log
# copy over static assets
COPY public public/


# Copy Nginx config template
COPY docker/web/config/nginx.conf /tmp/docker.nginx


# substitute variable references in the Nginx config template for real values from the environment
# put the final config in its place
RUN envsubst '$RAILS_ROOT' < /tmp/docker.nginx > /etc/nginx/conf.d/default.conf
EXPOSE 80

# Use the "exec" form of CMD so Nginx shuts down gracefully on SIGTERM (i.e. `docker stop`)
CMD [ "nginx", "-g", "daemon off;" ]