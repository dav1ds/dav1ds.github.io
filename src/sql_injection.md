#  Sql

## Definition: 

L'injection SQL (SQLi) est une vulnérabilité de sécurité qui permet à un attaquant d'interférer avec les requêtes qu'une application fait à sa base de données. 

Elle permet généralement à un pirate de visualiser des données qu'il n'est normalement pas en mesure d'extraire. 
Il peut s'agir de données appartenant à d'autres utilisateurs ou de toute autre donnée à laquelle l'application elle-même peut accéder. 

Dans de nombreux cas, un attaquant peut modifier ou supprimer ces données, ce qui entraîne des changements persistants dans le contenu ou le comportement de l'application.

Dans certains cas , le serveur ou l'infrastructure sous jacent peut etre compromis, jusqu'à effectuer des attaques de deni de service.

## Principe: 

Soit l'URL ( l'utilisateur clique sur lacategorie Cadeaux )

https://website.com/produits?categorie=Cadeaux

L'application effectue en arriere plan la requete ci-dessous sur la base de données

SELECT * FROM produits WHERE categorie = 'Cadeaux' AND disponibilité = 1

La requete indique de retourner 
- Tous les details 
- de la table produits
- de catégorie "Cadeaux"
- et de disponibilité  1 

disponibilité =1 permet d'afficher uniquement les produits disponible ( les produits non disponible auront la valeur 0 - disponibilité=0) 

L'application n'implemente aucune sécurité contre les injection SQL , il est donc possible de construire une requete de type

https://website.com/produits?categorie=Cadeaux'--

``` sql

SELECT * FROM produits WHERE categorie ='Cadeaux'--' AND released=1

```
Les caracteres -- sont interpreté comme un commentaire dans une requete SQL , le reste de la requete est ignoré, tous les produits ( disponible ou non ) sont affichés 

Il est possible d'utiliser d'autre combinaisons de caracteres pour aller plus loin 

https://website.com/produits?categories=Cadeaux'+OR+1=1--

```SQL
SELECT * FROM produits WHERE categorie = 'Cadeaux' OR 1=1--' AND disponible = 1
```
1=1 etant toujours vrai, la suite de la requête est ignorée

Ce type d'injection peut invalider la logique de l'application 

Exemple:
Cas d'une authentification d'un utilisateur avec son mot de passe :
SELECT * FROM users WHERE username = 'administrator'--' AND password = ''

 --> La connexion d'un utilisateur s'effectue sans mot de passe 

##  Recuperer des données d'une autre table de la base de données (UNION)

SELECT name, description FROM produits WHERE categorie= 'cadeaux'

l'ajout de ces caractères permet de récuperer les noms et mot de passe de tous les utilisateurs 

' UNION SELECT username, password FROM users--

##  Recuperer des information de la base de données ( Version , schema des tables, ... ) 
```
SELECT * FROM v$version
SELECT * FROM information_schema.tables
```


# Blind SQL injection : 

De nombreux cas d'injection SQL sont des vulnérabilités aveugles.
Cela signifie que l'application ne renvoie pas les résultats de la requête SQL ou les détails des erreurs de base de données dans ses réponses. 
Les vulnérabilités aveugles peuvent toujours être exploitées pour accéder à des données non autorisées, mais les techniques utilisées sont généralement plus compliquées et plus difficiles à mettre en œuvre.

...

# Detection des injections SQL :

L'injection SQL peut être détectée manuellement en utilisant un ensemble systématique de tests pour chaque point d'entrée de l'application.

- Soumission du caractère guillemet simple ' et recherche d'erreurs ou d'autres anomalies.
- soumettre une syntaxe SQL spécifique qui évalue la valeur de base (originale) du point d'entrée et une valeur différente, et rechercher les différences systématiques dans les réponses de l'application qui en résultent.
- Soumettre des conditions booléennes telles que OR 1=1 et OR 1=2, et rechercher les différences dans les réponses de l'application.
- Soumission de charges utiles conçues pour déclencher des délais lorsqu'elles sont exécutées dans le cadre d'une requête SQL, et recherche de différences dans le temps de réponse.
- Soumettre des charges utiles OAST conçues pour déclencher une interaction réseau hors bande lorsqu'elles sont exécutées dans le cadre d'une requête SQL, et surveiller les interactions qui en résultent.

Les injections sql peuvent etre réalisé sur les differentes partie d'une requete sql 

- Dans les instructions UPDATE, dans les valeurs mises à jour ou dans la clause WHERE.
- Dans les instructions INSERT, dans les valeurs insérées.
- Dans les instructions SELECT, dans le nom de la table ou de la colonne.
- Dans les instructions SELECT, dans la clause ORDER BY.

# Injection SQL dans d'autres contextes:

Les attaques par injection SQL peuvent être utiliser sur n'importe quelle entrée contrôlable qui est traitée comme une requête SQL par l'application.
Par exemple, certains sites web prennent des données au format JSON ou XML et les utilisent pour interroger la base de données.

Ces différents formats peuvent même fournir des moyens alternatifs pour obscurcir les attaques qui sont autrement bloquées par les WAFs et autres mécanismes de défense. 
Les implémentations faibles se contentent souvent de rechercher les mots-clés d'injection SQL courants dans la requête, de sorte que vous pouvez contourner ces filtres en encodant ou en échappant simplement les caractères dans les mots-clés interdits. 
Par exemple, l'injection SQL basée sur XML suivante utilise une séquence d'échappement XML pour encoder le caractère S dans SELECT :

```
<stockCheck>
    <productId>
        123
    </productId>
    <storeId>
        999 &#x53;ELECT * FROM information_schema.tables
    </storeId>
</stockCheck>
```

Le decodage s'effectuera par le serveur avant d'etre envoyé à la BDD

# Injection SQL de 2e ordre ou Injection SQL stocké (Stored sql injection )


l'application prend les données de l'utilisateur à partir d'une requête HTTP et les stocke en vue d'une utilisation ultérieure. 
Cela se fait généralement en plaçant les données dans la BDD, mais aucune vulnérabilité n'apparaît au moment où les données sont stockées. 
Plus tard, lors du traitement d'une autre requête HTTP, l'application récupère les données stockées et les incorpore dans une requête SQL de manière non sécurisée.

![stored sql injection](images/second-order-sql-injection.svg "Stored sql injection").

L'injection SQL de second ordre survient souvent dans des situations où les développeurs sont conscients des vulnérabilités de l'injection SQL et gèrent donc en toute sécurité le placement initial de l'entrée dans la base de données. Lorsque les données sont traitées ultérieurement, elles sont considérées comme sûres, puisqu'elles ont été placées dans la base de données en toute sécurité. À ce stade, les données sont manipulées de manière dangereuse, car le développeur les considère à tort comme fiables

# Specificité des BDD: 

Bien qu'il y ait des caratcteristiques communes dans l'utilisation du language SQL , il est necessaire de prendre en compte les specificités de chaque editeur 
Les differences peuvent etre sur :
- Les commentaires 
- la syntax pour concatener des chaines de caracteres 
- le regroupement de requetes 
- les messages d'erreur 
- la structure interne des tables ( nom des tables , des schemas etc )

Exemple:
Concatenation de chaine :

Oracle:	'foo'||'bar'
Microsoft:	'foo'+'bar'
PostgreSQL:	'foo'||'bar'
MySQL:	'foo' 'bar' [Note the space between the two strings]
        CONCAT('foo','bar')

Commentaires:

Oracle:	--comment
Microsoft:	--comment
            /*comment*/
PostgreSQL:	--comment
            /*comment*/
MySQL:	#comment
        -- comment [Note the space after the double dash]
        /*comment*/


# Mesure preventive contre les injections SQL

La plupart des cas d'injection SQL peuvent être évités en utilisant des requêtes paramétrées (prepared statement ) au lieu de la concaténation de chaînes de caractères dans la requête.

Le code suivant est vulnerable car les entrées utilisateur sont directement implementeés dans la requete 

```
String query = "SELECT * FROM produits WHERE categorie = '"+ input + "'";
Statement statement = connection.createStatement();
ResultSet resultSet = statement.executeQuery(query);
```

Elle peut etre réecrites de manière plus sûre. L'entrée de l'utilisateur ne vient pas interferer avec la structure de la requête

```
PreparedStatement statement = connection.prepareStatement("SELECT * FROM produits WHERE categorie = ?");
statement.setString(1, input);
ResultSet resultSet = statement.executeQuery();
```
Pour qu'une requête paramétrée soit efficace dans la prévention des injections SQL, la chaîne utilisée dans la requête doit toujours être une constante codée en dur et ne doit jamais contenir de données variables, quelle qu'en soit l'origine. 

