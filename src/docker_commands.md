# Lister les images 

> docker images

# Lister tous les conteneurs

> docker ps -a

# Telecharger une image si elle n'existe pas et lancer le conteneur 

> docker run __nom_image__

Démarrer un conteneur, se connecter avec un shell 

docker run -it __nom_du_conteneur__ /bin/bash

# Démarrer un conteneur existant

> docker start __nom ou id du conteneur__


# Désisntallation de docker 

```
sudo apt-get purge docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin docker-ce-rootless-extras

sudo rm -rf /var/lib/docker
sudo rm -rf /var/lib/containerd
```

[Voir la cheat-sheet/memo docker](images/docker-commands-cheat-sheet-pdf.pdf)
