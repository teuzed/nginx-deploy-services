FROM ubuntu:18.04


ENV DEBIAN_FRONTEND=noninteractive


RUN apt update -y \
    && apt install nginx curl vim -y \
    && apt-get install software-properties-common -y \
    && add-apt-repository ppa:certbot/certbot -y \
    && apt-get update -y \
    && apt-get install python-certbot-nginx -y \
    && apt-get clean


EXPOSE 80 443


STOPSIGNAL SIGTERM


COPY ./deploy/default.conf /etc/nginx/sites-available/default


COPY ./frontend/build /var/www/html


CMD ["nginx", "-g", "daemon off;"]

