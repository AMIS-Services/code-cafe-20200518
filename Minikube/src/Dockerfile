FROM nginx:alpine
LABEL maintainer="René Paul"
WORKDIR /usr/share/nginx/html
COPY . .
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]