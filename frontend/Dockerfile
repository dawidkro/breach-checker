# --- Etap 1: kontrola jakości HTML (narzędzia potrzebne tylko podczas budowania) ---
FROM node:24-alpine AS lint
LABEL maintainer="imie.nazwisko@example.com"
RUN npm install -g htmlhint@1.9.2
WORKDIR /site
COPY index.html style.css script.js ./
RUN htmlhint index.html

# --- Etap 2: obraz końcowy - sam nginx i pliki strony ---
# --- Etap 2: obraz końcowy - sam nginx i pliki strony ---
FROM nginx:alpine
# Łączy obraz w GHCR z repozytorium projektu (uprawnienia pakietu dziedziczone z repozytorium)
LABEL org.opencontainers.image.source="https://github.com/dawidkro/breach-checker"
LABEL maintainer="student.student@student.com"
COPY --from=lint /site/ /usr/share/nginx/html/
COPY nginx.conf /etc/nginx/conf.d/default.conf
EXPOSE 80