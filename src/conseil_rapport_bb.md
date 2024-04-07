# Conseil pour ecrire un bon rapport de vulnérabilités de BugBounty:

Ces conseils sont issus de l'article publié sur le site de YesWeHack:
https://www.yeswehack.com/fr/learn-bug-bounty/write-effective-bug-bounty-reports

Le score de gravité (CVSS) et la qualité du rapport vous permettent d'obtenir des points de bonus pour monter dans le classement des hunters, renforcer la confiance des équipes sécurité (client) et débloquer de nouvelles opportunité de gains ( ex: participation à des programmes privés,...)

##  Contrôle avant publication d'un rapport

- La vulnérabilité est-elle dans le périmètre du programme ? En tant que chasseur de bogues, vous devez respecter les règles du programme et ne chasser que sur les périmètres autorisés.

- Votre bogue fait-il partie de la liste des vulnérabilités admissibles du programme ? Une vulnérabilité non qualifiée risque d'entraîner le rejet du rapport et sa clôture en RTFS (Read The Fine Scope).

- Le bogue a-t-il un impact potentiel sur la sécurité ? En règle générale, la validation exige un impact direct sur la sécurité (sauf si la politique du programme en dispose autrement).

## Analyse de l'impact

L'impact d'une vulnérabilité influe directement sur la gravité de vos conclusions et, par conséquent, sur la récompense que vous pourriez recevoir.

Démontrez l'impact en :

- Quantifiant les dommages potentiels : Évaluez les données qui pourraient être compromises ou les fonctionnalités qui pourraient être utilisées de manière abusive, et dans quel but. 

Par exemple, la vulnérabilité pourrait-elle entraîner un accès non autorisé à des informations sensibles ou des pertes financières ? Quel est le scénario réaliste le plus défavorable ?

- Utiliser des scénarios du monde réel : Fournir un scénario d'attaque réaliste qui montre comment un attaquant pourrait exploiter la vulnérabilité. 
Cela aide l'équipe de sécurité du programme à comprendre les implications pratiques de la faille de sécurité.

Exemple de déclaration d'impact : "Cette vulnérabilité par injection SQL pourrait permettre à des pirates de récupérer l'ensemble de la base de données des utilisateurs, y compris des informations sensibles sur les clients, telles que les mots de passe et les numéros de carte de crédit.

## Analyse du comportement:

Rédigez une analyse de comportement qui explique comment vous avez identifié la vulnérabilité en incitant l'application ou le système à se comporter de manière anormale.

Par exemple :

- Comprendre le comportement attendu : Comprendre comment l'application ou le système est censé fonctionner en se basant sur la documentation, les cas d'utilisation et les interactions légitimes des utilisateurs.

- Identifier les anomalies : Montrer les écarts par rapport au comportement attendu qui indiquent une vulnérabilité, tels que des messages d'erreur inattendus, des fonctionnalités non documentées ou des réponses incohérentes de l'application aux entrées de l'utilisateur.

- Exemple d'analyse de comportement : "Au cours de l'analyse, il a été observé que la soumission d'une charge utile spécialement conçue dans le formulaire de connexion de l'utilisateur entraînait un retard inhabituel dans la réponse de l'application. Ce comportement est révélateur d'une vulnérabilité d'injection SQL basée sur le temps, car le temps de réponse de l'application variait de manière significative avec différentes charges utiles, ce qui suggère qu'elle exécutait des commandes SQL non autorisées."

## Rapport de recherche 

Sections du rapport:

- Description de la vulnérabilité

Description de l'énumération commune des faiblesses (Common Weakness Enumeration - CWE) liée à la vulnérabilité. Cela permet au lecteur d'avoir une compréhension générale de la vulnérabilité que vous avez découverte.

- Découverte de la vulnérabilité

Description du processus de découverte de la vulnérabilité.

- Preuve de concept (PoC)

Le PoC sert essentiellement à prouver l'existence de la vulnérabilité. L'une des meilleures façons d'y parvenir est de combiner un court texte récapitulatif avec une référence visuelle sous la forme d'une image et/ou d'une vidéo.

- Exploitation:

Une démonstration des étapes qu'un attaquant pourrait suivre pour exploiter la vulnérabilité. Ces étapes doivent être faciles à suivre afin que les triagers puissent reproduire votre exploit et valider votre rapport le plus rapidement possible.

Vous pouvez également inclure un script d'exploitation, ce qui est particulièrement pratique pour les vulnérabilités avancées comportant de nombreuses étapes complexes.

- L'impact:

Décrivez clairement l'impact de votre vulnérabilité et reliez-le à la preuve de concept.

- Remédiation:

Fournissez une solution technique pour résoudre la vulnérabilité. Ce n'est pas obligatoire, mais le fait de fournir une solution potentielle peut faire gagner du temps à l'équipe de sécurité et sera très apprécié.

- Références: 

Ajoutez des liens vers des ressources qui fournissent un contexte ou des informations supplémentaires qui aideront l'équipe de sécurité à comprendre la vulnérabilité. 

Par exemple, il peut s'agir d'URL pour la définition CWE pertinente et d'avis de sécurité pour des vulnérabilités existantes avec lesquelles votre bogue peut être enchaîné.
