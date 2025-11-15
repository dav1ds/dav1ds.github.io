# Contournement de la logique métier / failles logiques

Différentes des vulnérabilités techniques liées au code de l'application Web ou implementation de configuration
Plus difficile à identifier que les vulnérabilités techniques (XSS, injections SQL, CSRF…)

Principalement du à un manque de rigueur dans le design et d’un manque de tests approfondis d’un point de vue sécurité sur les processus métier de la solution.

## Méthodologie: 

Tester la sécurité logique nécessite d’aller plus loin que des outils de sécurité automatisés.
Ces failles sont découvertes lors des tests d’intrusion. 

Il est nécessaire d’étudier et de comprendre le fonctionnement prévu (et non prévu) d’un site ou d’une application et de penser « attaque » : quels gains pourraient obtenir des attaquants dans cette situation ?

__Détecter les failles logiques nécessite__ : 

- une compréhension complète du fonctionnement de l’application : règles métiers, processus… 

- découper et modéliser le workflow,

- identifier où sont effectués les contrôles, et où ils ne sont pas effectués,
d’analyser les contrôles effectués, les paramètres envoyés, les réponses retournées…


## Exemple: 

Ex1 : Attaque temporelle 

Sur un site de reservation en ligne de billet , l'application bloque temporairement la reservation de billets pour les autres clients, le temps de réaliser la transaction.
Une attaque possible  est de créer un script( ou manuellement via l'interface)  qui reserve tous les billets , empêchant tous les clients d'utiliser le site.
-> Deni de service pour les autres clients

Impact: financier, image de marque, perte de confiance 

Ex2: Attaque de contournement de workflow

Un site web de commerce électronique permettait aux utilisateurs de voir le produit et son prix, de sélectionner ce produit, d'acheter un résumé, puis de passer à la caisse. 
Le processus a été conçu pour être exécuté dans cet ordre précis uniquement. L'administrateur n'a pas défini de règles pour un ordre différent.


Un pirate a découvert qu'il pouvait retourner au panier d'achat après avoir injecté des prix personnalisés dans l'URL. Le serveur du site web l'a exécuté et a permis à l'attaquant de payer les prix révisés.

Ex3: Mauvais traitement de données 

Un site permet le transfert d'un montant entre 2 comptes

```php
$transferAmount = $_POST['amount'];
$currentBalance = $user->getBalance();

if ($transferAmount <= $currentBalance) {
    // Complete the transfer
} else {
    // Block the transfer: insufficient funds
}
```

Si l'attaquant insère un montant négatif dans le champ "tranferAmount" (-1000) 
Cette valeur est toujours inférieur à "currentBalance", le compte de l'attaquant peut être créditer  du montant (1000).






### Quelques grandes « catégories » de failles logiques : 

- les failles liées à un manque de robustesse dans un workflow, qui peut être contourné,
- les failles liées à un manque de contrôle sur les données, qui influencent les actions en cours d’exécution,
- les failles liées à une réaction inattendue de l’application,
- les failles liées à des attaques temporelles,
- les failles liées à un assemblage de fonctionnalités, qui deviennent problématiques uniquement une fois combinées.

## Remédiation: 

- Implémenter les contrôles adéquats à chaque étape de chaque workflow permet de s’assurer que la logique métier ne peut pas donner lieu à des abus ou être contournée

- Pentesteurs avec des compétences particulières pour ce type de faille
