Pentest web:

Methodo: 

1. Analyse de la surface d'attaque 

Analyse du site : Etudier le fonctionnement du site

Analyser les technos utilisées:( stack technique) 
-Serveur : editeur, version 
-framework: 
-language: javascript,php, autre 

ex outils: wapalyzer ( extensions navigateur) 

Cartographier de manière macro le site : 
Page principale
lien
formulaire
Enchainement des pages et des formulaires
sous-domaine
script
API
...

Outils: 
Burpsuite, Owasp Zap , Skipfish, w3af


Entetes de requetes/reponse
Methode : Post, GET, trace , Options , ...
Http/https

2. Identifier les points d'entrée 

repertoire (dirb) et fichiers ( cachés ou non) 
objet de formulaire (tri ,zone de texte, date , champs de mot de passe) 
URL

3. Analyser en details ( burpsuite , ZAP) 

Tester les points d'entrée avec differents paramètres
