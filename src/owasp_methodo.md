# Methodologie d'évaluation des risques OWASP

Objectifs d'une Methodologie d'evalutation des risques:

La mise en place d'un système d'évaluation des risques permet de gagner du temps et d'éviter les discussions sur les priorités. 

Ce système permettra de s'assurer que l'entreprise ne se laisse pas distraire par des risques mineurs tout en ignorant des risques plus graves qui sont moins bien compris.

Calculette en ligne : 

[https://owasp-risk-rating.com/](https://owasp-risk-rating.com/)

## Differentes méthodes:

- Classic Risk Rating: This risk rating methodology uses a Likelihood value and an Impact value with a mathematical formula applied to come up with a risk score.  Typically something like Risk = Likelihood x Impact.  I talk more about this risk scoring methodology in my Normalizing Risk Scores Across Different Methodologies blog post.

- CVSS: Also known as the Common Vulnerability Scoring System, CVSS is developed by the Forum of Incident Response and Security Teams (FIRST) organization and is what is used to rate all of the Common Vulnerabilities and Exposures (CVEs) found in the National Vulnerability Database (NVD).  It is comprised of a Base Vector, which has multiple values to estimate likelihood and impact, along with optional values to estimate the Temporal and Environmental impact on your environment.

- DREAD: The DREAD risk assessment model was initially used at Microsoft as a simple mnemonic to rate security threats on the basis of Damage, Reproducibility, Exploitability, Affected Users, and Discoverability.  We don't see it being used by customers very often, but it has been included in SimpleRisk since very early on in our product history.

- OWASP: The OWASP Risk Rating Methodology was created by Jeff Williams, one of the Founders of the OWASP organization, as a means to easily and more accurately assess the likelihood and impact of a web application vulnerability.  It's an application-centric play on the Classic Risk Rating described above, where the Likelihood is assessed based on Threat Agent and Vulnerability factors and the Impact is assessed based on Technical and Business factors.


2 Approche standard:

Modele de risque standard: 

**Risk = Likelihood * Impact** 

<ins>En francais</ins> :  
Risque = la probabilité d'exploitation d'une vulnerabilité * impacte sur le SI ou le business



La "probabilité" et "l'impact" sont décomposées pour déterminer ces facteurs de risque

**<ins>Etapes</ins>**: 

  1: Identifier un risque

  2: Facteurs d'estimation de la probabilité

  3: Facteurs d'estimation de l'impact

  4: Determiner la sévérité du risque

  5: Prioriser les remédiations

  6: Personnaliser votre modèle de risque


1. Identification des risques:

Le testeur doit rassembler des informations sur l'agent de menace concerné, l'attaque qui sera utilisée, la vulnérabilité impliquée et l'impact d'une exploitation réussie sur l'entreprise. 

Il peut y avoir plusieurs groupes d'attaquants possibles, ou même plusieurs impacts possibles sur l'entreprise.

En général, il est préférable de pécher par excès de prudence en choisissant l'option la plus défavorable, car c'est celle qui présente le risque global le plus élevé.

2. Facteurs d'estimation de la probabilité 

Au niveau le plus élevé, il s'agit d'une mesure approximative de la probabilité que cette vulnérabilité particulière soit découverte et exploitée par un attaquant.

Il n'est pas nécessaire d'être très précis sur cette estimation
Les niveaux "low", "medium" ou "high" sont suffisant


Un certain nombre de facteurs peuvent aider à déterminer la probabilité. La première série de facteurs est liée à l'agent de menace (Threat Agent) concerné. L'objectif est d'estimer la probabilité d'une attaque réussie à partir d'un groupe d'attaquants possibles.


### Agent de Menace ( Threat Agent ):

L'objectif est d'estimer la probabilité d'une attaque réussie par ce groupe d'agents de menace. Utilisez l'agent de menace le plus défavorable.

- Niveau de compétence(skill level): Quel est le niveau de compétence technique de ce groupe d'agents de menace ? 
    Aucune compétence technique (1), quelques compétences techniques (3), utilisateur avancé d'ordinateur (5), compétences en réseau et en programmation (6), compétences en matière de pénétration de la sécurité (9)

- Motivation (Motive): Quelle est la motivation de ce groupe d'agents de menace pour trouver et exploiter cette vulnérabilité ? 
    Récompense faible ou nulle (1), récompense possible (4), récompense élevée (9)

- Opportunité(Opportunity): Quelles sont les ressources et les opportunités nécessaires à ce groupe d'agents de lutte contre les menaces pour trouver et exploiter cette vulnérabilité ?
    Accès total ou ressources coûteuses nécessaires (0), accès ou ressources spéciales nécessaires (4), accès ou ressources partiels nécessaires (7), aucun accès ou ressources nécessaires (9).

- Taille: Quelle est la taille de ce groupe d'agents de menace ?          
    Développeurs (2), administrateurs système (2), utilisateurs de l'intranet (4), partenaires (5), utilisateurs authentifiés (6), utilisateurs anonymes de l'internet (9)

### Facteurs de vulnérabilité:
L'objectif est d'estimer la probabilité que la vulnérabilité en question soit découverte et exploitée


Facilité de découverte: Dans quelle mesure est-il facile pour ce groupe d'agents de découvrir cette vulnérabilité ? 
    Pratiquement impossible (1), difficile (3), facile (7), outils automatisés disponibles (9)

Facilité d'exploitation: Dans quelle mesure est-il facile pour ce groupe d'agents de menace d'exploiter cette vulnérabilité ?
    Théorique (1), difficile (3), facile (5), outils automatisés disponibles (9)

Niveau de diffusion (awareness) : Dans quelle mesure cette vulnérabilité est-elle connue de ce groupe d'agents de menace ?
    Inconnue (1), cachée (4), évidente (6), connue du public (9)

Détection des intrusions: Quelle est la probabilité qu'un exploit soit détecté ? 
    Détection active dans l'application (1), enregistrée et examinée (3), enregistrée sans examen (8), non enregistrée (9)

3. Facteur d'estimation de l'impact

2 types d'impact : Technique et  business 

L'impact sur l'entreprise est généralement le plus important.
Toutefois, il se peut que vous n'ayez pas accès à toutes les informations nécessaires pour déterminer les conséquences commerciales d'un exploit réussi. 
Dans ce cas, le fait de fournir autant de détails que possible sur le risque technique permettra au représentant approprié de l'entreprise de prendre une décision sur le risque commercial.

### Impact techniques:

L'objectif est d'estimer l'ampleur de l'impact sur le système en cas d'exploitation de la vulnérabilité.

- Perte de confidentialité: Quelle quantité de données pourrait être divulguée et quel est leur degré de sensibilité ?
    Divulgation d'un minimum de données non sensibles (2), divulgation d'un minimum de données critiques (6), divulgation d'un grand nombre de données non sensibles (6), divulgation d'un grand nombre de données critiques (7), divulgation de toutes les données (9).

- Perte d'intégrité: Quelle est la quantité de données susceptibles d'être corrompues et à quel point sont-elles endommagées ?
    Données minimales légèrement corrompues (1), données minimales gravement corrompues (3), données étendues légèrement corrompues (5), données étendues gravement corrompues (7), toutes les données totalement corrompues (9).

- Perte de disponibilité: Quelle quantité de service pourrait être perdue et quelle est son importance vitale ?
    Interruption minimale des services secondaires (1), interruption minimale des services primaires (5), interruption importante des services secondaires (5), interruption importante des services primaires (7), perte totale de tous les services (9).

- Tracabilité: Les actions des agents de menace peuvent-elles être rattachées à une personne ?
    Entièrement traçable (1), éventuellement traçable (7), complètement anonyme (9)

### Impact business: 

Le risque business justifie les investissement dans la résolution des problèmes de sécurité.

- Préjudice financier: Quel est le préjudice financier résultant d'un exploit ? 
    Moins que le coût de la correction de la vulnérabilité (1), effet mineur sur le bénéfice annuel (3), effet important sur le bénéfice annuel (7), faillite (9).

- Atteinte à la réputation: Un exploit entraînerait-il une atteinte à la réputation qui nuirait à l'entreprise ? 
    Dommage minime (1), perte de grands comptes (4), perte de clientèle (5), atteinte à la marque (9).

- Non-conformité: Quel est le degré d'exposition à la non-conformité ?
   Violation mineure (2), violation manifeste (5), violation très médiatisée (7)

- atteinte à la vie privée: Quelle quantité de données personnelss pourrait être divulguée ?
    Une personne (3), des centaines de personnes (5), des milliers de personnes (7), des millions de personnes (9).


4.  Determiner la sévérité du risque

![Risque generique](images/owasp-generic-level.png)


owasp-risk-rating-methodology.png
