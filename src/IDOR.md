# Insecure Direct Object Reference

Les références directes à des objets non sécurisées (IDOR) sont un type de vulnérabilité du contrôle d'accès qui se produit lorsqu'une application utilise des données fournies par l'utilisateur pour accéder directement à des objets.

Il ne s'agit que d'un exemple parmi les nombreuses erreurs de mise en œuvre du contrôle d'accès qui peuvent conduire au contournement des contrôles d'accès.

Les vulnérabilités IDOR sont le plus souvent associées à l'escalade horizontale des privilèges (privilèges équivalent à un autre utilisateur dans un contexte de sécurité different) mais elles peuvent également survenir en relation avec l'escalade verticale des privilèges (niveau supérieure de privilèges).

# Exemples: 

https://insecure-website.com/customer_account?customer_number=132355

La variable customer_number est directement utilisé comme index de la base de données ( en backend).
Si aucun controle n'est effectué, l'utilisateur peut simplement changer la valeur pour consulter les enregistrements d'autres clients.

Un attaquant pourrait être en mesure d'effectuer une escalade de privilèges horizontale et verticale en modifiant l'utilisateur pour qu'il dispose de privilèges supplémentaires tout en contournant les contrôles d'accès. 
Parmi les autres possibilités, citons l'exploitation d'une fuite de mot de passe ou la modification des paramètres une fois que l'attaquant a atterri sur la page des comptes de l'utilisateur.


Les vulnérabilités IDOR apparaissent souvent lorsque des ressources sensibles sont situées dans des fichiers statiques sur le système de fichiers côté serveur. Par exemple, un site web peut enregistrer les transcriptions de messages de chat sur le disque en utilisant un nom de fichier incrémentiel, et permettre aux utilisateurs de les récupérer en visitant une URL ( ex: https://insecure-website.com/static/12144.txt )