# Access Control / Authorization

## Modeles:

2 modeles sont largement utilisés : 	RBAC et Attribute-Based Access Control (ABAC).


- RBAC ( Role Base Access Control )

	- RBAC est un modèle de contrôle d'accès dans lequel l'accès est accordé ou refusé en fonction des rôles attribués à un utilisateur. Les autorisations ne sont pas directement attribuées à une entité ; elles sont plutôt associées à un rôle et l'entité hérite des autorisations de tous les rôles qui lui sont attribués. En général, la relation entre les rôles et les utilisateurs peut être multiple et les rôles peuvent être de nature hiérarchique.


- ABAC:

	- ABAC peut être défini comme un modèle de contrôle d'accès dans lequel "les demandes des sujets pour effectuer des opérations sur des objets sont accordées ou refusées en fonction des attributs attribués au sujet, des attributs attribués à l'objet, des conditions de l'environnement et d'un ensemble de politiques spécifiées en fonction de ces attributs et conditions" (NIST SP 800-162, pg. 7]). 

	Selon la définition de la NIST SP 800-162, les attributs sont simplement des caractéristiques représentées sous forme de paires nom-valeur et attribuées à un sujet, à un objet ou à l'environnement. 
	Le rôle professionnel, l'heure, le nom du projet, l'adresse MAC et la date de création ne sont qu'un très petit échantillon des attributs possibles qui soulignent la flexibilité des implémentations ABAC.

Exemple d'application qui utilise ce systeme:

	- Systèmes de gestion de contenu
	- ERP
	- Applications maison
	- Applications Web

Sur les bases de données relationnelles traditionnelles, les politiques ABAC peuvent contrôler l'accès aux données au niveau de la table, de la colonne, du champ, de la cellule et de la sous-cellule en utilisant des contrôles logiques avec des conditions de filtrage et de masquage basées sur des attributs.
	
Depuis Windows Server 2012, Microsoft a mis en place une approche ABAC pour contrôler l'accès aux fichiers et aux dossiers.

Le contrôle d'accès basé sur les attributs peut également être appliqué aux systèmes Big Data tels que Hadoop.


**Un 3e modèle, plus récent, qui gagne en popularité : Relationship-Based Access Control (ReBAC).**


- ReBAC (Relationship-Based Access Control) est un modèle de contrôle d'accès qui accorde l'accès en fonction des relations entre les ressources. 

	Par exemple, seul l'utilisateur qui a créé un message peut le modifier.
	Ceci est particulièrement nécessaire dans les applications de réseaux sociaux, comme Twitter ou Facebook, où les utilisateurs veulent limiter l'accès à leurs données (tweets ou posts) aux personnes qu'ils choisissent (amis, famille, followers).

Préférer le contrôle d'accès basé sur les attributs (ABAC) et les relations au contrôle d'accès basé sur les relations (ReBAC)  plutôt que RBAC


