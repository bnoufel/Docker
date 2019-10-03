FROM ruby

EXPOSE 3000

RUN apt-get update && apt-get install nodejs sqlite3 -y
RUN gem install rails
RUN mkdir -p /opt/app

ONBUILD COPY . /opt/app
ONBUILD WORKDIR /opt/app

# to run it
# docker build -t ft-rails:on-build
#  docker build -f Dockerfile-rails .
# docker run -ti -p 3000:3000 [ID]
