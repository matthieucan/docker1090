# partly taken from https://github.com/boxelio/dockerfiles/blob/master/flight/wiedehopf-tar1090/Dockerfile

FROM debian:buster-slim as staging

RUN apt update && apt upgrade -y
RUN apt install -y unzip wget

WORKDIR /tmp
RUN wget https://github.com/wiedehopf/tar1090/archive/master.zip
RUN unzip master.zip
RUN mv tar1090-master tar1090

FROM debian:buster-slim

RUN apt update && apt upgrade -y
RUN apt install -y jq nginx

RUN mkdir -p /var/www/tar1090/htdocs
COPY --from=staging /tmp/tar1090/html /var/www/tar1090/htdocs/
COPY --from=staging /tmp/tar1090/nginx.conf /etc/nginx/snippets/tar1090.conf.snippet
COPY --from=staging /tmp/tar1090/tar1090.sh /usr/local/bin/

# nginx
RUN sed -i "s#HTMLPATH#/var/www/tar1090/htdocs#g" /etc/nginx/snippets/tar1090.conf.snippet
RUN sed -i "s#/INSTANCE/#/#g" /etc/nginx/snippets/tar1090.conf.snippet
RUN sed -i "s#/INSTANCE#/#g" /etc/nginx/snippets/tar1090.conf.snippet
RUN sed -i "s#SOURCE_DIR#/run/dump1090#g" /etc/nginx/snippets/tar1090.conf.snippet
RUN rm /etc/nginx/sites-enabled/default
RUN echo "daemon off;" >> /etc/nginx/nginx.conf
COPY tar1090.nginx /etc/nginx/sites-enabled/

COPY config.js /var/www/tar1090/htdocs/

ENTRYPOINT ["nginx"]

# HISTORY_SIZE=1000 INTERVAL=60 CHUNK_SIZE=60 PTRACKS=24 tar1090.sh /run/dump1090 /run/dump1090
