FROM nginx:1.21.6

RUN rm /etc/nginx/conf.d/default.conf

COPY nginx.conf /etc/nginx/conf.d/default.conf

WORKDIR  /app

COPY .  /app

EXPOSE 8000

CMD ["nginx", "-g", "daemon off;"]