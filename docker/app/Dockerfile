FROM ruby:2.6.2
# Install dependencies
RUN apt-get update -qq && apt-get install -y build-essential libpq-dev nodejs

RUN apt-get -q clean

# Set an environment variable where the Rails app is installed to inside of Docker image:
ENV RAILS_ROOT /var/www/app_name
RUN mkdir -p $RAILS_ROOT
# Set working directory, where the commands will be ran:
WORKDIR $RAILS_ROOT
# Setting env up
ENV RAILS_ENV='production'
ENV RACK_ENV='production'

# Setting local
ENV LC_ALL C.UTF-8
# Setting timezone
ENV TZ Asia/Taipei
RUN cp /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

# Adding gems
COPY Gemfile Gemfile
COPY Gemfile.lock Gemfile.lock
RUN gem install bundler
RUN gem update
RUN gem uninstall fileutils
RUN gem update fileutils --default
RUN bundle install --jobs 20 --retry 5 --without development test --path vendor/bundle

# Adding project files
COPY . .
COPY ./docker/app/config/database.yml ./config/database.yml

# COPY ./docker/app/.env.example ./.env
# RUN bundle exec rake assets:precompile
RUN RAILS_ENV=$RAILS_ENV bundle exec rake assets:precompile
EXPOSE 3000
CMD ["bundle", "exec", "puma", "-C", "config/puma.rb"]