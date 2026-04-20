```bash
# 2.1 Écrivez index.html avec le contenu HTML minimal suivant (titre : "Ma première image Docker" , un <h1> avec votre prénom).

nano index.html

"""
<!DOCTYPE html>
<html>
<head>
    <title>Ma première image Docker</title>
</head>
<body>
    <h1>Bonjour, c'est Quentin !</h1>
</body>
</html>
"""

# 2.2 Écrivez un Dockerfile qui : - Part de l'image nginx:alpine, - Copie index.html dans /usr/share/nginx/html/index.html, - Expose le port 80

nano Dockerfile
"""
FROM nginx:alpine
COPY index.html /usr/share/nginx/html/index.html
EXPOSE 80
"""

# 2.3 Construisez l'image avec le tag mon-site:v1 .

docker build -t mon-site:v1 .

# 2.4 Lancez un conteneur basé sur cette image, en exposant le port 9090 → 80 , avec --rm . Vérifiez dans le navigateur.

docker run --rm -d -p 9090:80 mon-site:v1

# 2.5 Listez les images locales. Quelle est la taille de mon-site:v1 ? Comparez avec nginx:alpine .

docker images
(Presque la même taille)

"""
IMAGE          ID             DISK USAGE   CONTENT SIZE   EXTRA
mon-site:v1    f8ed04132141       92.6MB           26MB    U   
nginx:alpine   5616878291a2       93.5MB         26.9MB        
"""

# 2.6 Inspectez les layers de l'image avec docker history mon-site:v1 . Combien de layers ont été ajoutés par rapport à l'image de base ?

docker history mon-site:v1
(On voit copy et expose en plus)

"""
root@temp:~/exercice2# docker history mon-site:v1
IMAGE          CREATED              CREATED BY                                      SIZE      COMMENT
f8ed04132141   About a minute ago   EXPOSE [80/tcp]                                 0B        buildkit.dockerfile.v0
<missing>      About a minute ago   COPY index.html /usr/share/nginx/html/index.…   24.6kB    buildkit.dockerfile.v0
<missing>      4 days ago           RUN /bin/sh -c set -x     && apkArch="$(cat …   51.8MB    buildkit.dockerfile.v0
<missing>      4 days ago           ENV ACME_VERSION=0.3.1                          0B        buildkit.dockerfile.v0
<missing>      4 days ago           ENV NJS_RELEASE=1                               0B        buildkit.dockerfile.v0
<missing>      4 days ago           ENV NJS_VERSION=0.9.6                           0B        buildkit.dockerfile.v0
<missing>      4 days ago           CMD ["nginx" "-g" "daemon off;"]                0B        buildkit.dockerfile.v0
<missing>      4 days ago           STOPSIGNAL SIGQUIT                              0B        buildkit.dockerfile.v0
<missing>      4 days ago           EXPOSE map[80/tcp:{}]                           0B        buildkit.dockerfile.v0
<missing>      4 days ago           ENTRYPOINT ["/docker-entrypoint.sh"]            0B        buildkit.dockerfile.v0
<missing>      4 days ago           COPY 30-tune-worker-processes.sh /docker-ent…   16.4kB    buildkit.dockerfile.v0
<missing>      4 days ago           COPY 20-envsubst-on-templates.sh /docker-ent…   12.3kB    buildkit.dockerfile.v0
<missing>      4 days ago           COPY 15-local-resolvers.envsh /docker-entryp…   12.3kB    buildkit.dockerfile.v0
<missing>      4 days ago           COPY 10-listen-on-ipv6-by-default.sh /docker…   12.3kB    buildkit.dockerfile.v0
<missing>      4 days ago           COPY docker-entrypoint.sh / # buildkit          8.19kB    buildkit.dockerfile.v0
<missing>      4 days ago           RUN /bin/sh -c set -x     && addgroup -g 101…   5.59MB    buildkit.dockerfile.v0
<missing>      4 days ago           ENV DYNPKG_RELEASE=1                            0B        buildkit.dockerfile.v0
<missing>      4 days ago           ENV PKG_RELEASE=1                               0B        buildkit.dockerfile.v0
<missing>      4 days ago           ENV NGINX_VERSION=1.29.8                        0B        buildkit.dockerfile.v0
<missing>      4 days ago           LABEL maintainer=NGINX Docker Maintainers <d…   0B        buildkit.dockerfile.v0
<missing>      4 days ago           CMD ["/bin/sh"]                                 0B        buildkit.dockerfile.v0
<missing>      4 days ago           ADD alpine-minirootfs-3.23.4-x86_64.tar.gz /…   9.11MB    buildkit.dockerfile.v0
"""

# 2.7 Modifiez index.html (changez le <h1> ). Reconstruisez l'image avec le tag mon- site:v2 . Quelle étape a été rechargée depuis le cache ? Quelle étape a été réexécutée ?

nano index.html

L'étape FROM nginx:alpine a été rechargé depuis le cache (Docker n'a pas retéléchargé l'image).

L'étape COPY index.html a été réexécutée car Docker a détecté que le contenu du fichier index.html a changé. Toutes les étapes suivantes (comme le EXPOSE) sont également recréées.

"""
<!DOCTYPE html>
<html>
<head>
    <title>Ma premi  re image Docker</title>
</head>
<body>
    <h1>Aurevoir !!!</h1>
</body>
</html>
"""

# 2.8 Supprimez l'image mon-site:v1 (sans supprimer v2 ).
docker stop 293d107ce3aa
docker rmi mon-site:v1
```