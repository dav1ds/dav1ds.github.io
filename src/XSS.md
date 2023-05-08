# XSS (Cross Site Scripting Stored/Reflected)

Faille de sécurité qui permet à un attaquant d'injecter dans un site web un code client malveillant.

Ce code est exécuté par les victimes et permet aux attaquants de contourner les contrôles d'accès et d'usurper l'identité des utilisateurs.

Ces attaques réussissent si l'application Web n'emploie pas assez de validation ou d'encodage. Le navigateur de l'utilisateur ne peut pas détecter que le script malveillant n'est pas fiable et lui donne donc accès à tous les cookies, jetons de session ou autres informations sensibles propres au site, ou permet au script malveillant de réécrire le contenu HTML.

 - __Objectif :__

Détourner la logique d’une application web et permettre par exemple le vol de cookies ou de jetons de session, l’altération de données ou de contenus, l’exécution d’un malware, etc. Les possibilités d’exploitation d’une faille XSS sont nombreuses


# 3 Catégories:

- Attaques XXS stockées:

Le script injecté est stocké en permanence sur les serveurs cibles. La victime extrait ensuite ce script malveillant du serveur lorsque le navigateur envoie une demande de données.

- Les attaques XSS reflétées:

Lorsqu'un utilisateur est trompé en cliquant sur un lien malveillant, en soumettant un formulaire spécialement conçu ou en naviguant sur un site malveillant, le code injecté se rend sur le site Web vulnérable. 

Le serveur Web renvoie le script injecté au navigateur de l'utilisateur, par exemple dans un message d'erreur, un résultat de recherche ou toute autre réponse incluant des données envoyées au serveur dans le cadre de la demande. 
Le navigateur exécute le code car il suppose que la réponse provient d'un serveur "de confiance" avec lequel l'utilisateur a déjà interagi.

- Les attaques XSS basées sur DOM*

La charge utile est exécutée à la suite de la modification de l'environnement DOM (dans le navigateur de la victime) utilisé par le script d'origine côté client. 

La page elle-même ne change pas, mais le code côté client contenu dans la page s'exécute de manière inattendue en raison des modifications malveillantes apportées à l'environnement DOM.

La plupart du temps, les propriétés DOM telles que document.location, document.write et document.anchors sont utilisées pour lancer ce type d’attaque XSS. Cependant, cette vulnérabilité est plutôt rare car il est très difficile de l’identifier.


##  Remarque: 

Le scoring CVSS est principalement affecté par 

- Le changement de scope ( la vulnérabilité du composant web est transféré vers le navigateur du client) 

- Confidentialité : Low 
Les informations du navigateur de la victime associées au composant vulnérable peuvent être lues par le code JavaScript malveillant et envoyées à l'attaquant.

- Intégrité : Low 
Les informations du navigateur de la victime associées au composant vulnérable peuvent être modifiées par le code JavaScript malveillant.


# Remédiation:

Il faut partir du principe que les données recu de l'application Web ne sont pas toujours sûres.

- L’encodage (ou échappement) des données en entrée ou en sortie reste la mesure de sécurité essentielle pour prévenir les attaques XSS.

il s’agit de remplacer les caractères spéciaux par des valeurs encodées, de manière à ce que toute donnée saisie par un utilisateur soit traitée (reçue et interprétée) comme du texte, plutôt que comme du code.

- Filtrage des données reçues côté client. 

Cela signifie que l’ensemble des données doit passer par un filtre qui élimine les caractères dangereux comme la balise \<script\>, les gestionnaires d’événements HTML comme onActivate(), onClick(), les éléments JavaScript, etc.
Cette méthode fonctionne essentiellement pour les attaques XSS stockées.

- Valider les entrées utilisateurs:

Tous les champs de saisie correspondent au type de données attendu. 
Par exemple, pour un champ de saisie d’un numéro de téléphone, il doit être impossible pour un utilisateur d’insérer du texte.
De la même manière, les balises HTML n’étant pas légitimes dans ce type de formulaire, elles doivent être validées pour empêcher les attaquants de soumettre des scripts malveillants.

- CSP (Content Security Policy) développé par Mozilla

Ce standard de sécurité permet de contrôler (restreindre via autorisation) les sources externes (autres sites web) de récupération de données.

Si ce mécanisme est en place, le navigateur sera autorisé à accéder uniquement aux ressources figurant sur la liste blanche, en ignorant tous les autres domaines. Les scripts injectés ne seront donc pas exécutés même si un attaquant découvre des possibilités d’injection XSS.


# Ressources: 

[PayloadsAllTheThings](https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/XSS%20Injection)


# Annexes
*DOM: Document Object Model est une represention objet des données qui composent la structure et le contenu d'un document sur le web (HTML ou XML)
= Interface de programmation (independant de tout language) 


Pour rappel (non exhaustif), une XSS permet de réaliser des attaques de :

- Ad-Jacking, en injectant des publicités invasives afin de générer des revenus à l'insu du site légitime ;

- Click-Jacking, ajout d’une surcouche (overlay) cachée sur une page pour hijack (capturer / manipuler) des clics d'une victime ;

- Session Hijacking, en dérobant les cookies de session des victimes non protégées par le drapeau HttpOnly ;

- Content-Spoofing, en altérant le rendu des pages, le DOM, toutes les ressources côté client pour afficher un autre contenu ;

- Credential Harvesting, en modifiant le rendu pour afficher une fausse page/mire d'authentification en vue de dérober les crédentiels d'une victime ;

- Forced Downloads, pour forcer le téléchargement d'un fichier malicieux sur un domaine de confiance / légitime ;

- Crypto Mining, en exploitant les ressources du CPU des victimes afin de générer de la cryptomonnaie ;

- Anti-CSRF bypass, contournement de toutes les protections anti-CSRF (token dans des forms, en cookies, referer, etc.) ;

- Keylogging, en définissant des events JavaScript capturant les frappes du clavier sur la page impactée ;

- Recording audio, avec les évolutions de JavaScript et HTML5, il est possible d'accéder au microphone de la victime et d'enregistrer / espionner les échanges vocaux (nécessite une autorisation) ;

- Taking pictures, accès à la webcam de la victime et espionnage (nécessite une autorisation) ;

- Geo-location, accès à la géolocalisation de la victime (nécessite une autorisation) ;

- Stealing HTML5 web storage data, une XSS permet d'atteindre et de dérober le contenu des nouveaux lieux de stockage locaux de données utilisés par les applications modernes, dont window.localStorage() et window.webStorage() ;

- Browser & System Fingerprinting, récupération du browser name, version, plugins installés et leurs versions, système d'exploitation, architecture, heure, langue, résolution d'écran, etc. ;

- Network scanning, via une XSS un attaquant a la main sur le site chargé dans le navigateur d'une victime, donc il se situe dans le réseau de la victime et peut scanner des ranges d'IP et ports à sa convenance (via JavaScript) [BEE] ;

- Crashing Browser, simplement pour nuire à des victimes, en réalisant des traitements très consommateurs en ressources ou via des exploits DoS destinés à une version précise ;

- Stealing information, récupération d'informations sensibles / personnelles sur une page web authentifiée sous le compte d'une victime, et les transmettre au serveur de l'attaquant ;

- Redirecting, une XSS permet la redirection d'une victime et donc engendre une Open-Redirect ;

- Tab-napping, en détectant l'activité d'une victime (pas de clic ni de touche clavier durant 1 minute, la victime a quitté son poste). En conséquence il est possible de remplacer la page courante par une autre à son insu ;

- Capturing screenshot, via HTML5 Canvas, il est possible de simuler la prise d'un screenshot complet de la page web courante (très employé avec les Blind-XSS) ;

- Perform Actions, l'attaquant ayant la main sur le navigateur, celui-ci peut réaliser toutes les actions souhaitées dans le contexte de la victime. Sur un réseau social : poster des messages, XSS-worm, etc.

- Black-SEO, avec les « bots » tels que le Google-Bot qui reposent sur des navigateurs headless (et donc interprète le JavaScript), il est possible de nuire au ranking et référencement d'un site web en injectant et référençant des URL comprenant des XSS qui injectent du contenu nuisible (Viagra, etc.) ;

- Brute-force / user-enumeration distributed, en exploitant une XSS et des faiblesses au niveau des CORS, un attaquant peut profiter de toutes les victimes de son XSS pour brute-forcer un espace authentifié tiers ou réaliser une énumération de manière distribuée avec la multitude d'adresses IP que lui fournissent ses victimes.
