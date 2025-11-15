# Pollution par prototype

La pollution des prototypes est une vulnérabilité qui se produit dans les langages qui utilisent l'héritage basé sur les prototypes, tels que JavaScript. Cette vulnérabilité permet à un attaquant de manipuler ou de modifier de manière inappropriée l'objet global d'une application, ce qui affecte tous les objets qui héritent des propriétés de ce prototype.

F12 ( outils dev de navigateur)  

```JavaScript
>let monobjet={};
>Object.getPrototypeOf(monobjet);
constructor
: 
ƒ Object()
hasOwnProperty
: 
...
ƒ isPrototypeOf()
propertyIsEnumerable
...
__proto__
: 
(...)
get __proto__
: 
ƒ __proto__()
set __proto__
: 
ƒ __proto__()

```

L'objet créé  herite automatiquement des prototypes internes 

Par défaut, JavaScript attribue automatiquement aux nouveaux objets l'un de ses prototypes internes. Les objets héritent automatiquement de toutes les propriétés du prototype qui leur est attribué, à moins qu'ils ne possèdent déjà leur propre propriété avec la même clé.


"__proto__" est une propriété qui renvoie le "prototype" de la classe de l'objet. 
Cette propriété sert de getter et de setter pour le prototype de l'objet. 

Cela signifie q'il est possible de lire le prototype et ses propriétés, et même les réaffecter si nécessaire.
En raison de la signification particulière de __proto__ dans un contexte JavaScript, l'opération de fusion peut affecter les propriétés imbriquées au prototype de l'objet plutôt qu'à l'objet cible lui-même. 
Par conséquent, l'attaquant peut polluer le prototype avec des propriétés contenant des valeurs nuisibles, qui peuvent ensuite être utilisées par l'application de manière dangereuse.

## Comment cette vulenrabilité peut-elle etre exploiter ? 

L'idée générale qui sous-tend la pollution des prototypes est que l'attaquant doit contrôler au moins les paramètres "key1" et "value" d'une expression de ce type :

```JavaScript
obj[key1][key2] = value

```

Les vulnérabilités liées à la pollution des prototypes surviennent généralement lorsqu'une fonction JavaScript fusionne de manière récursive un objet contenant des propriétés contrôlables par l'utilisateur avec un objet existant, sans avoir au préalable assaini les clés.

En raison de la signification particulière de __proto__ dans un contexte JavaScript, l'opération de fusion peut affecter les propriétés imbriquées au prototype de l'objet au lieu de l'objet cible lui-même. Par conséquent, l'attaquant peut polluer le prototype avec des propriétés contenant des valeurs nuisibles, qui peuvent ensuite être utilisées par l'application de manière dangereuse.




## Clés de réussite d'un exploitation de cette vulenrabilités:

- Une source de pollution prototype: Il s'agit de toute entrée qui vous permet d'empoisonner des objets prototypes avec des propriétés arbitraires. 
- Un puit (sink) : En d'autres termes, une fonction JavaScript ou un élément DOM qui permet l'exécution de code arbitraire.
- Un gadget exploitable: Il s'agit de toute propriété qui est passée dans un puits sans filtrage ou assainissement approprié.


Sources: 
Une source de pollution des prototypes est toute entrée contrôlable par l'utilisateur qui permet d'ajouter des propriétés arbitraires aux objets prototypes. 

- Url, 
- entrée basée sur JSON
- messages web.


Bien que la pollution des prototypes soit souvent inexploitable en tant que vulnérabilité autonome, elle permet à un attaquant de contrôler des propriétés d'objets qui seraient autrement inaccessibles. Si l'application traite ensuite une propriété contrôlée par l'attaquant d'une manière dangereuse, elle peut potentiellement être associée à d'autres vulnérabilités. 

Les vulnérabilités liées à la pollution des prototypes peuvent conduire à une autre vulnérabilité connue sous le nom de DOM XSS dans le cas d'une exploitation coté client
OU execution de code à distance (RCE) dans le cas d'une exploitation coté serveur


Exemple (coté client) :
```html
https://redacted.com/?__proto__[evilProperty]=alert(1)
```

L'operationd e fusion va assigner à l'objet et à ses prototype parent la valeur de "evilProperty"

```
targetObject.__proto__.evilProperty = 'payload';
```


En conséquence, evilProperty est assigné à l'objet prototype renvoyé plutôt qu'à l'objet cible lui-même.

En pratique, l'injection d'une propriété appelée evilProperty n'aura probablement aucun effet. Cependant, un attaquant peut utiliser la même technique pour polluer le prototype avec des propriétés utilisées par l'application ou toute bibliothèque importée.


-----------
Un gadget permet de transformer une vulnerabilité par pollution de prototype en un véritable exploit. Il s'agit de toute propriété qui est :

- Utilisé par l'application d'une manière dangereuse, par exemple en le transmettant à un puits (sink) sans filtrage ou assainissement approprié. 
- Contrôlable par l'attaquant via la pollution du prototype. En d'autres termes, l'objet doit pouvoir hériter d'une version malveillante de la propriété ajoutée au prototype par un attaquant.


## Recherche coté client de source vulnerable par pollution de prototype

1. Manuellement

Il faut tester par  essai/erreurs un point d'entrée ( URL) ou en envoyant du JSON

exemple : 

```html
vulnerable-website.com/?__proto__[foo]=bar 
ou avec notation "dot ."
vulnerable-website.com/?__proto__.foo=bar
```
Dans la console de votre navigateur, inspectez Object.prototype pour voir si vous avez réussi à le polluer avec votre propriété arbitraire :

```
Object.prototype.foo
// "bar" indicates that you have successfully polluted the prototype
// undefined indicates that the attack was not successful
```


2. Dom invader de Burpsuite

DOM Invader est capable de tester automatiquement les prototypes de sources de pollution lorsque vous naviguez, ce qui peut vous faire gagner beaucoup de temps et d'efforts.

3. Trouver un gadget qui peut etre exploité 



## Recher et exploitation coté serveur:

Plus difficile à trouver ( aucun acces au code serveur), il faut proceder à l'aveugle
Si une vulnerabilités est exploité , le serveur peut etre dans un état instable , voir inoperrant ( necessite un redémarrage du serveur) 


## Comment limiter cette vulnérabilités

The ECMAScript 5 dynamic permet de "geler" le prototype

```JavaScript
- Object.freeze(Object.prototype);
- Object.freeze(Object);
- ({}).__proto__.test = 123
- ({}).test // undefined
```

Il est également possible de definir un objet sans prototype en utilisant la fonction Object.create()

```JavaScript
- var obj = Object.create(null);
- obj.__proto__ //undefined
- obj.constructor //undefined
```



