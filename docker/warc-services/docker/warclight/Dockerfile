
FROM ruby:2.5.3

RUN curl -sL https://deb.nodesource.com/setup_14.x | bash - && \
    apt-get install -y nodejs

RUN gem install warclight

RUN rails new /app -m https://raw.githubusercontent.com/archivesunleashed/warclight/main/template.rb

WORKDIR /app
RUN bundle install

CMD rails s -b 0.0.0.0