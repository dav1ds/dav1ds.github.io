# Lister les images 

docker images

#Lister tous les conteneurs

docker ps -a

# lancer (et telecharger une image ) un conteneur 

docker run __nom_image__

Démarrer un conteneur, se connecter avec un shell 

docker run -it __nom_du_conteneur__ /bin/bash

# Démarrer un conteneur existant
# docker start __nom ou id du conteneur__


# Désisntallation de docker 

sudo apt-get purge docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin docker-ce-rootless-extras

sudo rm -rf /var/lib/docker
sudo rm -rf /var/lib/containerd


Voir la cheat-sheet : [docker-commands-cheat-sheet-pdf.pdf]
