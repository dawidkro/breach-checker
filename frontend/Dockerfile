FROM nginx:alpine
LABEL maintainer="prometeuszzzx@gmail.com"
LABEL autor="Dawid Kropiewnicki" data="2026-10-23"
RUN apk add --no-cache nodejs npm && npm install -g htmlhint
COPY . /usr/share/nginx/html
RUN date > /usr/share/nginx/html/build.txt
EXPOSE 80