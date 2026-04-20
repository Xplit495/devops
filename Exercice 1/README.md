```bash
# 1.1 Téléchargez l'image nginx:alpine depuis Docker Hub sans lancer de conteneur.

docker pull nginx:alpine

# 1.2 Lancez un conteneur nginx:alpine nommé mon-nginx en arrière-plan, en exposant le port 8080 de votre machine sur le port 80 du conteneur.

docker run -d --name mon-nginx -p 8080:80 nginx:alpine

# 1.3 Vérifiez que le conteneur tourne. Quelle commande permet de lister uniquement les conteneurs en cours d'exécution ?

docker ps
"""
root@temp:\~# docker ps
CONTAINER ID   IMAGE          COMMAND                  CREATED         STATUS         PORTS                                     NAMES
b58210e80326   nginx:alpine   "/docker-entrypoint.…"   6 seconds ago   Up 4 seconds   0.0.0.0:8080->80/tcp, \[::]:8080->80/tcp   mon-nginx
"""

# 1.4 Ouvrez http://localhost:8080 dans votre navigateur (ou avec curl ). Que voyez- vous ?

"""
Welcome to nginx!

If you see this page, nginx is successfully installed and working. Further configuration is required for the web server, reverse proxy, API gateway, load balancer, content cache, or other features.

For online documentation and support please refer to nginx.org.
To engage with the community please visit community.nginx.org.
For enterprise grade support, professional services, additional security features and capabilities please refer to f5.com/nginx.

Thank you for using nginx.
"""

# 1.5 Affichez les logs du conteneur mon-nginx .

docker logs mon-nginx
"""
root@temp:\~# docker logs mon-nginx
/docker-entrypoint.sh: /docker-entrypoint.d/ is not empty, will attempt to perform configuration
/docker-entrypoint.sh: Looking for shell scripts in /docker-entrypoint.d/
/docker-entrypoint.sh: Launching /docker-entrypoint.d/10-listen-on-ipv6-by-default.sh
10-listen-on-ipv6-by-default.sh: info: Getting the checksum of /etc/nginx/conf.d/default.conf
10-listen-on-ipv6-by-default.sh: info: Enabled listen on IPv6 in /etc/nginx/conf.d/default.conf
/docker-entrypoint.sh: Sourcing /docker-entrypoint.d/15-local-resolvers.envsh
/docker-entrypoint.sh: Launching /docker-entrypoint.d/20-envsubst-on-templates.sh
/docker-entrypoint.sh: Launching /docker-entrypoint.d/30-tune-worker-processes.sh
/docker-entrypoint.sh: Configuration complete; ready for start up
2026/04/20 16:26:22 \[notice] 1#1: using the "epoll" event method
2026/04/20 16:26:22 \[notice] 1#1: nginx/1.29.8
2026/04/20 16:26:22 \[notice] 1#1: built by gcc 15.2.0 (Alpine 15.2.0)
2026/04/20 16:26:22 \[notice] 1#1: OS: Linux 6.17.13-2-pve
2026/04/20 16:26:22 \[notice] 1#1: getrlimit(RLIMIT\_NOFILE): 1024:524288
2026/04/20 16:26:22 \[notice] 1#1: start worker processes
2026/04/20 16:26:22 \[notice] 1#1: start worker process 30
2026/04/20 16:26:22 \[notice] 1#1: start worker process 31
2026/04/20 16:26:22 \[notice] 1#1: start worker process 32
192.168.123.90 - - \[20/Apr/2026:16:28:10 +0000] "GET / HTTP/1.1" 200 896 "-" "Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:149.0) Gecko/20100101 Firefox/149.0" "-"
2026/04/20 16:28:10 \[error] 30#30: \*1 open() "/usr/share/nginx/html/favicon.ico" failed (2: No such file or directory), client: 192.168.123.90, server: localhost, request: "GET /favicon.ico HTTP/1.1", host: "192.168.123.2:8080", referrer: "http://192.168.123.2:8080/"
192.168.123.90 - - \[20/Apr/2026:16:28:10 +0000] "GET /favicon.ico HTTP/1.1" 404 153 "http://192.168.123.2:8080/" "Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:149.0) Gecko/20100101 Firefox/149.0" "-"
"""

# 1.6 Arrêtez le conteneur mon-nginx sans le supprimer. Puis listez tous les conteneurs (y compris arrêtés). Quelle est la différence avec la commande de la question 1.3 ?

docker stop mon-nginx
docker ps -a
(Les ports ne sont pas binds)

"""
root@temp:\~# docker ps -a
CONTAINER ID   IMAGE          COMMAND                  CREATED         STATUS                      PORTS     NAMES
b58210e80326   nginx:alpine   "/docker-entrypoint.…"   3 minutes ago   Exited (0) 17 seconds ago             mon-nginx
"""

# 1.7 Supprimez le conteneur mon-nginx . Vérifiez qu'il n'existe plus.

docker rm mon-nginx

"""
root@temp:\~# docker ps -a
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
"""

# 1.8 Quelle commande aurait permis de lancer le conteneur de façon à ce qu'il soit automatiquement supprimé à l'arrêt ?

docker run --rm -d --name mon-nginx -p 8080:80 nginx:alpine
```