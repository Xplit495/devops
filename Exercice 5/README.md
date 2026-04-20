```bash

5.4 Construisez l'image flask-app:v1.



docker build -t flask-app:v1 .



5.5 Lancez un conteneur en passant la variable d'environnement APP\_ENV=production et en exposant le port 5000 . Vérifiez / et /health dans le navigateur ou avec curl.



docker run -d -p 5000:5000 -e APP\_ENV=production flask-app:v1



5.6 Relancez le conteneur sans passer APP\_ENV . Quelle valeur s'affiche ? D'où vient-elle ?



docker run -d -p 5000:5000 flask-app:v1



5.7 Quelle est la taille de l'image flask-app:v1 ? Que pourrait-on faire pour la réduire davantage (donnez deux pistes) ?



docker images flask-app:v1

\# Pour réduire on pourrait faire un multi-stage build ou utiliser une image python3.12 avec alpine (connu pour être léger).

```

