FROM nginx:1.21

COPY nginx.conf /etc/nginx/conf.d/default.conf
COPY docker-environment /usr/local/bin/docker-environment

ENV FPM_HOST fpm
ENV FPM_PORT 9000
ENV APP_ROOT /var/www/html

ENTRYPOINT ["/usr/local/bin/docker-environment"]
CMD ["nginx", "-g", "daemon off;"]
