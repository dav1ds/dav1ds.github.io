# HSTS : HTTP strict transport Security 

HTTP strict transport security header utilise 2 directives :
- max-age: Indique le nombre de second  que le navigateur doit utiliser pour convertir les requetes HTTP en HTTPS.

- includeSubDomains: Indique tout les sous domaines relatifs au HTTPS.
 
- preload ( Non officiel ): Indique que le domain est prechargé sur une liste et que les navigateurs ne doivent se connecter qu'en HTTPS
Supporté par la plupart des navigateurs mais ne fait pas partie de la spec officiel  (See hstspreload.org for more information.)


Exemple d'implementation sz l'entete HTSTS :

Strict-Transport-Security: max-age=31536000; includeSubDomains

Comment tester : 
curl -s -D- https://owasp.org | grep -i strict
