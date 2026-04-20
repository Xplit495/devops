```bash
# 3.1 Lancez un conteneur alpine en mode interactif ( -it ) avec --rm . À l'intérieur, créez le fichier /data/test.txt avec le contenu "bonjour" . Quittez ( exit ). Relancez un nouveau conteneur alpine . Le fichier existe-t-il ? Expliquez pourquoi.

docker run -it --rm alpine sh

mkdir /data
echo "bonjour" > /data/test.txt
exit

docker run -it --rm alpine sh

cat /data/test.txt

"""
cat: can't open '/data/test.txt': No such file or directory
"""

Le fichier n'existe plus parce que le système de fichiers du conteneur est éphémère. Avec l'option --rm, le premier conteneur a été complètement détruit en le quittant, emportant ses données avec lui.

# 3.2 — Bind mount : Créez un dossier exercice-3/html/ sur votre machine. Placez-y unfichier index.html . Lancez un conteneur nginx:alpine en montant ce dossier dans /usr/share/nginx/html avec -v . Modifiez index.html sur votre machine (sans redémarrer le conteneur) et rafraîchissez le navigateur. Que constatez-vous ?

mkdir -p exercice3/html
echo "<h1>Version 1</h1>" > exercice3/html/index.html

docker run -d --name nginx-bind -p 8080:80 -v $(pwd)/exercice-3/html:/usr/share/nginx/html nginx:alpine

echo "<h1>Version 2 modifiée en direct</h1>" > exercice-3/html/index.html

Si on va sur http://localhost:8080, on constate que la modification est immédiate sans avoir besoin de redémarrer le conteneur. Le dossier du conteneur est un simple "miroir" du dossier de notre machine.

docker rm -f nginx-bind

# 3.3 — Volume nommé : Créez un volume Docker nommé mes-donnees .

docker volume create mes-donnees

# 3.4 Lancez un conteneur alpine avec --rm en montant mes-donnees sur /data . Dans le conteneur, créez /data/persistant.txt avec le contenu "je survis" . Quittez.

docker run -it --rm -v mes-donnees:/data alpine sh

echo "je survis" > /data/persistant.txt
exit

# 3.5 Lancez un nouveau conteneur alpine (différent du précédent) avec le même volume monté. Le fichier /data/persistant.txt existe-t-il ? Qu'est-ce que cela démontre ?

docker run -it --rm -v mes-donnees:/data alpine sh

cat /data/persistant.txt

"""
je survis
"""

Oui, le fichier existe et contient bien "je survis" ! Cela démontre que les volumes nommés permettent de conserver la persistance des données indépendamment du cycle de vie des conteneurs.

# 3.6 Listez les volumes Docker existants. Où Docker stocke-t-il physiquement ce volume sur votre machine ?

docker volume ls

"""
root@temp:~/exercice3/html# docker volume ls
DRIVER    VOLUME NAME
local     mes-donnees
"""

Sur une machine Linux, Docker stocke physiquement les données de ce volume dans le répertoire : /var/lib/docker/volumes/mes-donnees/_data.

# 3.7 Supprimez le volume mes-donnees . Quelle précaution faut-il prendre avant de le supprimer ?

docker volume rm mes-donnees
```