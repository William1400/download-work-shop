# Présentation de TypeScript
TypeScript est un langage  à typage statique conçu par Anders Heljsberg (également concepteur du langage C#). Son but est de rendre plus fiable et facile l'écriture du code en JavaScript. Le code en TS sera compilé en JS. (comme SCSS pour CSS)


- [Présentation de TypeScript](#présentation-de-typescript)
  - [Quels sont les avantages de TS?](#quels-sont-les-avantages-de-ts)
  - [Quels sont les désavantages de TS?](#quels-sont-les-désavantages-de-ts)
  - [À quel moment utiliser TypeScript ?](#à-quel-moment-utiliser-typescript-)
  - [Installation de TypeScript](#installation-de-typescript)
- [tsconfig](#tsconfig)
- [Typage](#typage)
  - [Type](#type)
  - [Interface](#interface)
  - [Type vs Interface ?](#type-vs-interface-)
- [Classe](#classe)
- [Compilation de votre code](#compilation-de-votre-code)

<br>

## Quels sont les avantages de TS?
- Limites les erreurs
- Meilleure autocomplétion et documentation
- Cible la version de compilation du code TS en JS (ES5,ES6, etc)


<br>

## Quels sont les désavantages de TS?
- Outil supplémentaire
- L'écosystème JavaScript
- Perte en flexibilité (mais gagne en prévisibilité)
- Code moins lisible

<br>

## À quel moment utiliser TypeScript ?
TS permet d'apporter une rigueur dans votre code et sa prévisibilité permet d'éviter les bugs. Son ciblage permet de nous passer d'outil comme "Babel" par exemple. C'est pourquoi nous vous conseillons d'utiliser TS pour de gros projets.

<br>

## Installation de TypeScript

Il y a deux façons principales d'obtenir le TypeScript disponible pour votre projet :

- Via npm (le gestionnaire de paquets Node.js)
- En installant les plugins Visual Studio de TypeScript

Visual Studio 2017 et Visual Studio 2015 Update 3 incluent la prise en charge du langage TypeScript par défaut mais n'incluent pas le compilateur TypeScript, tsc. Si vous n'avez pas installé TypeScript avec Visual Studio, vous pouvez toujours le télécharger.

Pour les utilisateurs de npm :
<br>

```js
npm install -g typescript
```
<br>
<br>

# tsconfig
La présence d'un fichier tsconfig.json dans un répertoire indique que ce dernier est la racine d'un projet TypeScript. Le fichier tsconfig.json spécifie les fichiers racine et les options de compilation nécessaires pour compiler le projet.

Exemple d'un fichier **tsconfig.json**:


```tsx
{
  "compilerOptions": {
    "module": "commonjs",
    "noImplicitAny": true,
    "removeComments": true,
    "preserveConstEnums": true,
    "sourceMap": true
  },
  "files": [
    "core.ts",
    "sys.ts",
    "types.ts",
    "scanner.ts",
    "parser.ts",
    "utilities.ts",
    "binder.ts",
    "checker.ts",
    "emitter.ts",
    "program.ts",
    "commandLineParser.ts",
    "tsc.ts",
    "diagnosticInformationMap.generated.ts"
  ]
}
```
**compilerOptions**: Ces options constituent la majeure partie de la configuration et couvrent la manière dont le langage doit fonctionner.

**module**: Définit le système de modules pour le programme.

**noImplicitAny**: En activant noImplicitAny, TypeScript émettra toutefois une erreur chaque fois qu'il en aura déduit une.

**removeComments**: Supprime tous les commentaires des fichiers TypeScript lors de la conversion en JavaScript. La valeur par défaut est false.

**preserveConstEnums**: N'efface pas les déclarations const enum dans le code généré. Les const enum permettent de réduire l'empreinte mémoire globale de votre application au moment de l'exécution en émettant la valeur de l'enum au lieu d'une référence.

**sourceMap**: Permet la génération de fichiers sourcemap. Ces fichiers permettent aux débogueurs et autres outils d'afficher le code source TypeScript original lorsqu'ils travaillent avec les fichiers JavaScript émis.


Ces paramètres vous permettent d'avoir plus de contrôles sur votre code. Il en existe beaucoups d'autres
Vous pourrez trouver de la documentation à ce sujet [ici](https://www.typescriptlang.org/docs/handbook/tsconfig-json.html)

<br>
<br>

# Typage

Le principal apport du langage TypeScript est la possibilité d'associer un type à une donnée.

```
let result: number;
let message: string;
let response: boolean;
let obj: any;
```
- **number** : pour les nombres entiers ou flottants.
- **string** : pour les chaînes de caractères.
- **boolean** : pour les booléens donc true ou false.
- **any** : type par défaut attribuer par TS à une variable globale s'il ne reconnait pas son type lors de sa déclaration.

Le typage peut aussi se faire implicitement:
``let message = "Hello World"  // message sera un string``

<br>

## Type
Il est possible de créer des variables de types:

```
type Message = string;

let message: Message;

```

Vous pouvez aussi créer des **unions** de types:

```
type Message = string | null

let message: Message;
```

**message** peut donc être un string *ou* null.

Créer ces variables rendra votre code plus lisible

Les types peuvent se faire sous forme d'objet

<br>

## Interface
L'interface est une autre façon de typer un objet

```
interface{
  lastname: string,
  firstname: string,
  age: number,

}
```
<br>

## Type vs Interface ?
Quelle est la différence entre les types et les interfaces ?
Les types sont plus rigides que les interfaces car une fois déclarer aucune extension n'est posssible.

Exemple:

*Ce code provoquera une erreur*

```
type Person = {
  lastname: string,
  firstname: string,
}

type Person = {
  age: number
}

```
Si nous souhaitons rajouter plus loin la variable **age** dans notre objet nous ne pouvons pas y faire référence à un autre endroit dans le code avec *type*. Or c'est possible avec les interfaces.

Dans cette situation, préférez donc ceci:

```
interface Person{
  lastname: string,
  firstname: string,
}

interface Person{
  age: number
}
```

Notre interface **Person** a déjà été déclarer, elle sera reconnu par TS et implémentera **age**.


<br>
<br>

# Classe

Le JavaScript traditionnel utilise des fonctions et l'héritage basé sur des prototypes pour construire des composants réutilisables, mais cela peut sembler un peu gênant pour les programmeurs plus à l'aise avec une approche orientée objet, où les classes héritent de fonctionnalités et où les objets sont construits à partir de ces classes. À partir d'ECMAScript 2015, également connu sous le nom d'ECMAScript 6, les programmeurs JavaScript peuvent construire leurs applications en utilisant cette approche orientée objet basée sur les classes. Dans TypeScript, nous permettons aux développeurs d'utiliser ces techniques dès maintenant, et de les compiler en JavaScript qui fonctionne sur tous les principaux navigateurs et plates-formes, sans avoir à attendre la prochaine version de JavaScript.

Prenons un exemple simple basé sur les classes :

```typescript
class Greeter {
  greeting: string;
 
  constructor(message: string) {
    this.greeting = message;
  }
 
  greet() {
    return "Hello, " + this.greeting;
  }
}
 
let greeter = new Greeter("world");

```

La syntaxe devrait vous sembler familière si vous avez déjà utilisé C# ou Java. Nous déclarons une nouvelle classe Greeter. Cette classe possède trois membres : une propriété appelée greeting, un constructeur et une méthode greet.

Vous remarquerez que dans la classe, lorsque nous faisons référence à l'un des membres de la classe, nous ajoutons this. Cela indique qu'il s'agit d'un accès membre.

Dans la dernière ligne, nous construisons une instance de la classe Greeter en utilisant new. Cela fait appel au constructeur que nous avons défini plus tôt, en créant un nouvel objet avec la forme Greeter, et en exécutant le constructeur pour l'initialiser.


<br>
<br>

#  Compilation de votre code

Il faut maintenant compiler votre code en Javascript.
Pour celà lancez le compilateur TypeScript dans la ligne de commande :

``tsc greeter.ts``

Le résultat sera un fichier greeter.js qui contient le même JavaScript que celui que vous avez entré. Nous sommes prêts à utiliser TypeScript dans notre application JavaScript !

[React TypeScript](./react-TS.md)