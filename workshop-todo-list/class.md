# #5: 💼 Class

Une class est une structure programmatique spéciale. Elle est définie avec des membres qui peuvent être des propriétés \(variables\) et des méthodes \(fonctions\). Ensuite, des instances de la classe sont créées, généralement en appelant l'opérateur `new` sur la classe: `let myInstance = new myClass();`. L'instance créée est un objet sur lequel vous pouvez appeler les méthodes de la classe et obtenir et définir les valeurs de ses propriétés. Plusieurs instances peuvent être créées à partir d'une classe.

## Dans Angular...

Angular se charge de créer des instances des classes que vous définissez - si elles sont reconnues comme des blocs de construction Angular. Les décorateurs font cette connexion avec Angular.

A chaque fois que vous utilisez un composant dans un modèle, une nouvelle instance de celui-ci est créée. Par exemple, ici trois instances de la classe InputButtonUnitComponent seront créées:

{% code title="src/app/app.component.ts" %}
```markup
// example only

template: `
  <app-input-button-unit></app-input-button-unit>
  <app-input-button-unit></app-input-button-unit>
  <app-input-button-unit></app-input-button-unit>
`,
```
{% endcode %}

La class `InputButtonUnitComponent` est vide. Avant d'ajouter des membres \(propriétés et méthodes\), nous allons présenter le constructeur, qui n'est pas écrit dans la classe du composant par défaut.

## constructor

Le constructeur est une méthode qui est appelée par JavaScript lorsqu'une instance de la classe est créée. Tout ce qui se trouve à l'intérieur de cette méthode est utilisé pour créer l'instance. Il peut recevoir des paramètres et exécuter une certaine logique pour définir les valeurs des propriétés de l'instance créée.

> Une fonctionnalité forte dans Angular qui utilise le constructeur est l'injection de dépendance. Nous y reviendrons plus tard, lorsque nous commencerons à utiliser des services.

Une classe peut avoir des méthodes avec différents noms, mais le mot réservé pour cette méthode spéciale est `constructor`. Pour utiliser le constructeur d'une classe, implémentez-le simplement:

```typescript
class MyClass {
// members can be defined and initiated here

  constructor(/* parameters can be defined here */) {
    // initialization code here
  }
}

```

Plusieurs constructeurs peuvent être écrits avec différents ensembles d'arguments \(paramètres\). Lors de la création d'une instance d'une classe, les paramètres requis doivent être passés.

Par exemple, la classe `Date` a plusieurs constructeurs. Pour créer un objet `Date`, vous pouvez l'appeler sans paramètres pour créer un objet de la date et de l'heure actuelles:

```typescript
const now = new Date();
```

ou avec un paramètre, par exemple une chaîne représentant une date pour créer un objet avec cette valeur:

```typescript
const ninetyFive = new Date('1995-12-17T03:24:00');
```

## Propriétés

La propriété `title` que nous avons ajoutée est utilisée pour stocker une valeur, dans notre cas de type chaîne de caractères. Chaque instance de la classe aura sa propre propriété `title`, ce qui signifie que vous pouvez changer la valeur de `title` dans une instance, mais elle restera la même dans les autres instances.

Avec TypeScript, nous devons déclarer les membres de la classe soit dans le corps de la classe en dehors de toute méthode, soit les passer au constructeur - comme nous le verrons lorsque nous utiliserons des services.

Nous pouvons déclarer une propriété sans l'initialiser:

```typescript
title: string;
```

Alors vous pouvez attribuer une valeur à une étape ultérieure, par exemple dans le constructeur. Ici, nous avons explicitement noté que `title` est du type `string`. \(Le type est inféré par TypeScript lorsque nous attribuons immédiatement une valeur, donc il n'est pas nécessaire d'ajouter le type dans ce cas.\)

Quand on fait référence à un membre de la classe à partir d'une méthode de la classe, on doit le préfixer avec `this`. C'est une propriété spéciale qui pointe vers l'instance actuelle.

Essayez de définir une valeur différente pour `title` à partir du constructeur. Voir le résultat dans le navigateur:

{% code title="src/app/input-button-unit/input-button-unit.component.ts" %}
```typescript
title = 'Hello World';

constructor() { 
  this.title = 'I Love Angular';
}
```
{% endcode %}

### Méthodes

Ajoutons une méthode qui change la valeur de `title` en fonction de l'argument que nous allons passer. Nous l'appellerons `changeTitle`. La méthode aura un paramètre de type `string`. Ajoutez-le **à l'intérieur du corps de la classe** \(mais pas à l'intérieur d'une autre méthode\):

{% code title="src/app/input-button-unit/input-button-unit.component.ts" %}
```typescript
changeTitle(newTitle: string) {
  this.title = newTitle;
}
```
{% endcode %}

**Note:** Les fonctions et les méthodes peuvent renvoyer une valeur qui peut être utilisée lorsque la méthode est appelée. Par exemple:

{% code title="code for example" %}
```typescript
function multiply (x: number, y: number) {
  return x * y;
}

let z = multiply(4, 5);
console.log(z);
```
{% endcode %}

La méthode `changeTitle` n'est pas encore utilisée. Nous pouvons l'appeler à partir d'une autre méthode ou du modèle \(que nous verrons dans les chapitres suivants\). Appelons-le à partir du constructeur.

{% code title="src/app/input-button-unit/input-button-unit.component.ts" %}
```typescript
constructor() { 
  this.changeTitle('My First Angular App');
}
```
{% endcode %}

## Astuce de debugging

Vous pouvez toujours utiliser `console.log(someValue)` à l'intérieur des méthodes de classe. Alors la valeur que vous avez passée comme argument sera imprimée dans la console du navigateur. De cette façon, vous pouvez voir l'ordre d'exécution des méthodes et la valeur de l'argument que vous passez \(s'il s'agit d'une variable\). Par exemple:

{% code title="src/app/input-button-unit/input-button-unit.component.ts" %}
```typescript
constructor() { 
  console.log('in constructor');
  this.changeTitle('My First Angular App');
  console.log(this.title);
}

changeTitle(newTitle: string) {
  console.log(newTitle);
  this.title = newTitle;
}
```
{% endcode %}

La console du navigateur fait partie de ses outils de développement. Vous pouvez voir comment ouvrir la console dans différents navigateurs ici : [https://webmasters.stackexchange.com/questions/8525/how-do-i-open-the-javascript-console-in-different-browsers](https://webmasters.stackexchange.com/questions/8525/how-do-i-open-the-javascript-console-in-different-browsers)

{% hint style="info" %}
💾 **Save your code to GitHub**

StackBlitz users - press **Save** in the toolbar and continue to the next section of the tutorial.

Commit all your changes by running this command in your project directory.

```
git add -A && git commit -m "Your Message"
```

Push your changes to GitHub by running this command in your project directory.

```
git push
```
{% endhint %}

{% hint style="success" %}
[See the results on StackBlitz](https://stackblitz.com/github/ng-girls/todo-list-tutorial/tree/master/examples/0\_05-class)
{% endhint %}
