FROM ubuntu:18.04 
RUN apt-get update && \
    apt-get install -y nginx  

COPY ./nginx.conf /etc/nginx/nginx.conf
ENTRYPOINT ["nginx"]
CMD ["-g", "daemon off;"]
WORKDIR /etc/nginx/
EXPOSE 80
VOLUME /var/lib/nginx
