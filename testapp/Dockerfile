FROM rails:5

RUN apt-get update -qq && apt-get install -y build-essential libpq-dev nodejs

ENV RAILS_ROOT /home
RUN mkdir -p $RAILS_ROOT

WORKDIR $RAILS_ROOT
