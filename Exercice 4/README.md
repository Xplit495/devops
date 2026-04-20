```bash

\# 4.1 Listez les réseaux Docker existants sur votre machine. Quels sont les trois réseaux créés par défaut ?



docker network ls



\# 4.2 Créez un réseau bridge personnalisé nommé mon-reseau.



docker network create mon-reseau



\# 4.3 Lancez un conteneur nginx:alpine nommé serveur-web connecté à mon-reseau , en arrière-plan.



docker run -d --name serveur-web --network mon-reseau nginx:alpine



\# 4.4 Lancez un conteneur alpine nommé client connecté à mon-reseau en mode interactif. Depuis client , effectuez un wget -qO- http://serveur-web . Que récupérez- vous ? Pourquoi peut-on utiliser le nom serveur-web plutôt qu'une adresse IP ?



docker run -it --name client --network mon-reseau alpine

\# dans le conteneur :

wget -qO- http://serveur-web



\# => page HTML de nginx

\# Pourquoi ? : DNS auto sur les réseaux bridges personnalisées



\# 4.5 Quittez client . Lancez un nouveau conteneur alpine nommé client-externe sans le connecter à mon-reseau (réseau par défaut). Essayez de joindre serveur-web par son nom. Que se passe-t-il ? Pourquoi ?



docker run -it --name client-externe alpine

\# wget http://serveur-web -> echec : pas de DNS sur un bridge par défaut



\# 4.6 Quelle commande permet de connecter client-externe à mon-reseau après son démarrage ?



docker network connect mon-reseau client-externe



\# 4.7 Nettoyez : arrêtez et supprimez tous les conteneurs créés dans cet exercice, puis supprimez mon-reseau



docker stop serveur-web client client-externe

docker rm serveur-web client client-externe

docker network rm mon-reseau

```

