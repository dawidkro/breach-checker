docker inspect:
PS C:\Users\Dawid\Desktop\labmw\breach-checker> docker inspect --format '{{json .Config.Labels}}' dawid-frontend:v1
{"autor":"Dawid Kropiewnicki","data":"2026-10-23","maintainer":"prometeuszzzx@gmail.com"}
docker history dawid-frontend:v1:
IMAGE          CREATED              CREATED BY                                      SIZE      COMMENT
c45f3deda65f   About a minute ago   EXPOSE [80/tcp]                                 0B        buildkit.dockerfile.v0
<missing>      About a minute ago   RUN /bin/sh -c htmlhint /usr/share/nginx/htm…   4.1kB     buildkit.dockerfile.v0
<missing>      About a minute ago   COPY . /usr/share/nginx/html # buildkit         32.8kB    buildkit.dockerfile.v0
<missing>      About a minute ago   RUN /bin/sh -c apk add --no-cache nodejs npm…   102MB     buildkit.dockerfile.v0
<missing>      About a minute ago   LABEL autor=Dawid Kropiewnicki data=2026-10-…   0B        buildkit.dockerfile.v0
<missing>      About a minute ago   LABEL maintainer=prometeuszzzx@gmail.com        0B        buildkit.dockerfile.v0
<missing>      30 hours ago         RUN /bin/sh -c set -x     && apkArch="$(cat …   52.4MB    buildkit.dockerfile.v0
<missing>      30 hours ago         ENV ACME_VERSION=0.4.1                          0B        buildkit.dockerfile.v0
<missing>      30 hours ago         ENV NJS_RELEASE=1                               0B        buildkit.dockerfile.v0
<missing>      30 hours ago         ENV NJS_VERSION=1.0.2                           0B        buildkit.dockerfile.v0
<missing>      31 hours ago         CMD ["nginx" "-g" "daemon off;"]                0B        buildkit.dockerfile.v0
<missing>      31 hours ago         STOPSIGNAL SIGQUIT                              0B        buildkit.dockerfile.v0
<missing>      31 hours ago         EXPOSE map[80/tcp:{}]                           0B        buildkit.dockerfile.v0
<missing>      31 hours ago         ENTRYPOINT ["/docker-entrypoint.sh"]            0B        buildkit.dockerfile.v0
<missing>      31 hours ago         COPY 30-tune-worker-processes.sh /docker-ent…   16.4kB    buildkit.dockerfile.v0
<missing>      31 hours ago         COPY 20-envsubst-on-templates.sh /docker-ent…   12.3kB    buildkit.dockerfile.v0
<missing>      31 hours ago         COPY 15-local-resolvers.envsh /docker-entryp…   12.3kB    buildkit.dockerfile.v0
<missing>      31 hours ago         COPY 10-listen-on-ipv6-by-default.sh /docker…   12.3kB    buildkit.dockerfile.v0
<missing>      31 hours ago         COPY docker-entrypoint.sh / # buildkit          8.19kB    buildkit.dockerfile.v0
<missing>      31 hours ago         RUN /bin/sh -c set -x     && addgroup -g 101…   5.75MB    buildkit.dockerfile.v0
<missing>      31 hours ago         ENV DYNPKG_RELEASE=1                            0B        buildkit.dockerfile.v0
<missing>      31 hours ago         ENV PKG_RELEASE=1                               0B        buildkit.dockerfile.v0
<missing>      31 hours ago         ENV NGINX_VERSION=1.31.8                        0B        buildkit.dockerfile.v0
<missing>      31 hours ago         LABEL maintainer=NGINX Docker Maintainers <d…   0B        buildkit.dockerfile.v0
<missing>      31 hours ago         CMD ["/bin/sh"]                                 0B        buildkit.dockerfile.v0
<missing>      31 hours ago         ADD alpine-minirootfs-3.24.3-x86_64.tar.gz /…   9.08MB    buildkit.dockerfile.v0


Do zadanie03.md: wynik docker inspect z etykietami, wynik docker history <twoje-imie>-frontend:v1 oraz 2-3 zdania:

Od EXPOSE do LABEL maintainer, to  Dockerfile. Wszystko niżej to nginx:alpine. Dwa ostatnie wiersze to Alpine, na którym nginx jest zbudowany.0B mają instrukcje, które nie ruszają plików, tylko zapisują coś w konfiguracji obrazu np LABEL, ENV nie tworzą warstwy z plikami, zostawiają tylko wpis w historii. W inspect widać tylko wartość maintainer, bo etykiety działają jak słownik cyzli LABEL ustawił ten sam klucz co LABEL z nginx:alpine i nadpisał jego wartość, a inspect pokazuje końcową konfigurację obrazu, podczas gdy history wypisuje wszystkie instrukcje po kolei.


Docker PS:
PS C:\Users\Dawid\Desktop\labmw\breach-checker> docker ps
CONTAINER ID   IMAGE               COMMAND                  CREATED              STATUS              PORTS                                     NAMES
0b5f3f5dd970   dawid-frontend:v2   "/docker-entrypoint.…"   About a minute ago   Up About a minute   0.0.0.0:8082->80/tcp, [::]:8082->80/tcp   build2
47b67384a03c   dawid-frontend:v2   "/docker-entrypoint.…"   6 minutes ago        Up 6 minutes        0.0.0.0:8081->80/tcp, [::]:8081->80/tcp   build1

docker exec build1 cat:
Fri Oct 23 06:12:43 UTC 2026
docker exec build2 cat:
Fri Oct 23 06:12:43 UTC 2026
Czas jes taki sam bo RUN wykonał się raz, podczas docker build, a jego wynik, czyli plik z datą, zapisał się w warstwie obrazu jak każdy inny plik. RUn bierze gotowe warstwy i uruchamia tylko polecenie startowe, więc oba kontenery widzą ten sam, zamrożony plik.


root   /usr/share/nginx/html
Mówi nginx, że pliki strony ma brać z katalogu /usr/share/nginx/html, dlatego właśnie tam kopiujemy pliki aplikacji instrukcją COPY

PS C:\Users\Dawid\Desktop\labmw\breach-checker> docker exec zly ls /usr/share/nginx
html
index.html
script.js
style.css
PS C:\Users\Dawid\Desktop\labmw\breach-checker> docker exec zly ls /usr/share/nginx/html
50x.html
build.txt
index.html
RUN htmlhint /usr/share/nginx/html/index.html sprawdza plik pod konkretną ścieżką, a nie „to, co skopiował COPY”. Pod tą ścieżką leży teraz domyślna strona nginx z obrazu bazowego, a nie Twoja.


PS C:\Users\Dawid\Desktop\labmw\breach-checker> docker rmi breach-checker-frontend:latest
Untagged: breach-checker-frontend:latest
PS C:\Users\Dawid\Desktop\labmw\breach-checker> docker images
                                                                                                    i Info →   U  In Use
IMAGE                                               ID             DISK USAGE   CONTENT SIZE   EXTRA
breach-checker-frontend:v2                          efd030c70c79        229MB         59.5MB
dawid-frontend:latest                               c45f3deda65f        229MB         59.5MB
dawid-frontend:v1                                   c45f3deda65f        229MB         59.5MB
dawid-frontend:v2                                   d8f616e36a0e        229MB         59.5MB    U
dawid-frontend:zla-sciezka                          f441f6826faf        229MB         59.5MB    U
metody-wirtualizacji-wyklad-3-kod-backend:latest    bb4d92541799        212MB         51.7MB
metody-wirtualizacji-wyklad-3-kod-frontend:latest   763cee2b3e90       93.6MB         26.3MB
metody-wirtualizacji-wyklad-3-kod-sensor:latest     76278d0ad95c        177MB         43.3MB
mw-projekt-restauracja-backend:latest               21da0e603136        222MB         53.7MB    U
mw-projekt-restauracja-frontend:latest              d4cfa4b21816        793MB          183MB    U
redis:7-alpine                                      858f009f9709       57.8MB         16.7MB    U
PS C:\Users\Dawid\Desktop\labmw\breach-checker> docker rmi breach-checker-frontend:v2
Untagged: breach-checker-frontend:v2
Deleted: sha256:efd030c70c79c60d66b2e1d7f3f14c4afae48dd7905b1c47efd9626376535a79
PS C:\Users\Dawid\Desktop\labmw\breach-checker>

Plik z dwiema nazwami znika z dysku dopiero po usunięciu ostatniej z nich. Tak samo docker rmi z tagiem najpierw zdejmuje nazwę (Untagged). Sam obraz usuwa (Deleted) dopiero wtedy, gdy nie wskazuje na niego żaden tag i nie używa go żaden kontener, nawet zatrzymany. Jeśli jakiś kontener go używa, rmi odmówi z błędem conflict. Nawet przy Deleted znikają tylko warstwy, których nie używa żaden inny obraz.