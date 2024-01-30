# #7: 📤Event binding

Nous voulons que notre application réagisse aux actions de l'utilisateur. Nous voulons mettre à jour le titre de la tâche à chaque fois que l'utilisateur le modifie, ou ajouter une nouvelle tâche lorsque l'utilisateur appuie sur le bouton Enregistrer ou sur la touche Entrée.

Nous n'avons toujours pas toute la liste à afficher, mais pour le moment nous allons utiliser une autre façon de tester l'action. Nous changerons pour la bonne fonctionnalité plus tard.

Le composant `input-button-unit` devrait ressembler à ceci:

{% code title="src/app/input-button-unit/input-button-unit.component.ts" %}
```typescript
import { Component } from '@angular/core';
import { CommonModule } from '@angular/common';

@Component({
  selector: 'app-input-button-unit',
  standalone: true,
  imports: [CommonModule],
  templateUrl: './input-button-unit.component.html',
  styleUrls: ['./input-button-unit.component.scss']
})
export class InputButtonUnitComponent {
  title = 'Hello World';
}
```
{% endcode %}

Le composant `input-button-unit` devrait ressembler à ceci:

{% code title="src/app/input-button-unit/input-button-unit.component.html" %}
```markup
    <p>
      input-button-unit works!
      The title is: {{ title }}
    </p>

    <input [value]="title">
    <button>Save</button>
```
{% endcode %}

## Implémenter la méthode changeTitle

Tout d'abord, implémentons `changeTitle`. Il recevra le nouveau titre comme argument :

{% code title="src/app/input-button-unit/input-button-unit.component.ts" %}
```typescript
changeTitle(newTitle: string): void {
  this.title = newTitle;
}
```
{% endcode %}

## Event binding

Tout comme la liaison aux propriétés des éléments, nous pouvons lier aux événements émis par les éléments. Encore une fois, Angular nous donne un moyen facile de le faire. 
Il suffit d'entourer le nom de l'événement avec des parenthèses, et de lui passer la méthode qui doit être exécutée lorsque l'événement est émis.

Essayons un exemple simple, où le titre est modifié lorsque l'utilisateur clique sur le bouton. Remarquez les parenthèses autour de `click`.

{% code title="src/app/input-button-unit/input-button-unit.component.html" %}
```markup
  <p>
    input-button-unit works!
    The title is: {{ title }}
  </p>

  <input [value]="title">
  <button (click)="changeTitle('Button Clicked!')">
    Save
  </button>
```
{% endcode %}

> L'événement s'appelle `click` et non `onClick` - en Angular, vous supprimez le préfixe `on` des événements dans les éléments.

Vérifiez le résultat dans le navigateur : cliquez sur le bouton Save.

## Event Data

Nous passons une string statique à l'appel de la méthode: `Button Clicked!`. Mais nous voulons passer la valeur que l'utilisateur a tapée dans la zone de saisie !

Dans le prochain chapitre, nous apprendrons à utiliser les propriétés d'un élément dans un autre élément dans le même modèle. Ensuite, nous pourrons terminer la mise en œuvre de l'événement de clic du bouton `Save`. Mais maintenant, nous allons lier une méthode à un événement sur l'élément d'entrée: lorsque l'utilisateur clique sur Entrée, la méthode `changeTitle` sera appelée.

### Evénement 'keyup'

Quand l'utilisateur tape, des événements clavier sont émis. Par exemple `keydown` et `keyup`. Nous allons attraper l'événement `keyup` (lorsqu'une touche enfoncée est relâchée) et changer le titre:

{% code title="src/app/input-button-unit/input-button-unit.component.html" %}
```markup
<input [value]="title" (keyup)="changeTitle('Button Clicked!')">
```
{% endcode %}

Maintenant lorsque l'utilisateur tape dans la zone de saisie, le titre est changé en `Button Clicked!`.

**Tip:** Quand un élément HTML devient long en raison de ses attributs, vous pouvez le rendre plus facile à lire en le divisant en plusieurs lignes :

{% code title="src/app/input-button-unit/input-button-unit.component.html" %}
```markup
<input [value]="title"
       (keyup)="changeTitle('Button Clicked!')">
```
{% endcode %}

### L'objet $event

Maintenant nous réagissons lorsque l'événement `keyup` se produit. Angular nous permet d'obtenir l'objet événement lui-même. Il est passé à la liaison d'événement en tant que `$event` - nous pouvons donc l'utiliser lorsque nous appelons `changeTitle()`.

L'objet événement émis sur les événements `keyup` a une référence à l'élément qui a émis l'événement - l'élément d'entrée. La référence est conservée dans la propriété `target` de l'événement. Comme nous l'avons vu précédemment, l'élément d'entrée a une propriété `value` qui contient la string actuelle qui se trouve dans la zone d'entrée. Nous pouvons obtenir la valeur actuelle de l'élément d'entrée en utilisant `$event.target.value.`

Cependant, si vous essayez de le faire directement dans le modèle, comme indiqué ci-dessous, vous rencontrerez un problème.

{% code title="src/app/input-button-unit/input-button-unit.component.html" %}
```html
<input [value]="title"
       (keyup)="changeTitle($event.target.value)"><!-- ca ne marche pas -->
```
{% endcode %}

TypeScript ne peut pas être sûr que `$event.target` est un élément d'entrée. Il pourrait s'agir de n'importe quel type d'élément. Et la plupart des éléments n'ont pas de membre `value`.

Nous devons dire à TypeScript que nous savons que l'objet `$event.target` est de type `HTMLInputElement`, par opposition à `EventTarget`. Le _casting_ est fait avec le mot-clé `as`. Cependant, il ne peut pas être fait dans le modèle. Si vous essayez de caster dans le modèle, comme indiqué ci-dessous, vous obtiendrez une erreur.

{% code title="src/app/input-button-unit/input-button-unit.component.html" %}
```html
<input [value]="title"
       (keyup)="changeTitle(($event.target as HTMLInputElement).value)"><!-- ca ne marche pas -->
```
{% endcode %}

La solution consiste à effectuer le _casting_ dans une méthode de classe. Comme nous voulons que la méthode `changeTitle` soit générique afin qu'elle puisse être utilisée également dans d'autres endroits (comme le bouton), elle doit recevoir une string. Nous ne l'utiliserons donc pas pour recevoir l'événement et le caster.

Nous allons écrire une autre méthode qui ne sera utilisée que pour le _casting_. C'est aussi la solution suggérée par le tutoriel Angular : [Event Binding Concepts](https://angular.io/guide/event-binding-concepts).

Ajoutez la méthode suivante dans la classe du composant, avant ou après la méthode `changeTitle`.

{% code title="src/app/input-button-unit/input-button-unit.component.ts" %}
```typescript
  getInputValue(event: Event): string {
    return (event.target as HTMLInputElement).value;
  }
```
{% endcode %}

Maintenant ajustez la méthode passée à l'événement `(keyup)` pour qu'elle utilise à la fois `changeTitle` et `getInputValue` comme ceci:

{% code title="src/app/input-button-unit/input-button-unit.component.html" %}
```html
<input [value]="title"
       (keyup)="changeTitle(getInputValue($event))">
```
{% endcode %}

Vérifiez le résultat dans le navigateur. Maintenant, à chaque frappe de touche, vous pouvez voir que le titre change et reflète la valeur d'entrée.

### Appuyer sur la touche Entrée

Vous pouvez limiter le changement à une seule frappe de touche spéciale, dans notre cas c'est la touche Entrée. Angular nous facilite vraiment la tâche. L'événement `keyup` a des propriétés qui sont des événements plus spécifiques. Il suffit donc d'ajouter le nom de la touche à laquelle vous souhaitez écouter - dans notre cas, c'est `keyup.enter` :

{% code title="src/app/input-button-unit/input-button-unit.component.html" %}
```html
<input [value]="title"
       (keyup.enter)="changeTitle(getInputValue($event))">
```
{% endcode %}

Maintenant le titre ne changera que lorsque l'utilisateur appuiera sur la touche Entrée.

### Explorer $event

![lab-icon](<../assets/lab (2).jpg>)**Playground:** Vous pouvez modifier la méthode `getInputValue` pour enregistrer l'objet `$event` dans la console. De cette façon, vous pouvez l'explorer et voir ses propriétés.

Changez la méthode `getInputValue`:

{% code title="src/app/input-button-unit/input-button-unit.component.ts" %}
```typescript
getInputValue(event): string {
  console.log(event);
  return (event.target as HTMLInputElement).value;
}
```
{% endcode %}

Essayez-le !

N'oubliez pas de supprimer `console.log(event);` avant de continuer.

Vos fichier devraient ressembler à ceci :

{% code title="src/app/input-button-unit/input-button-unit.component.ts" %}
```typescript
import { Component } from '@angular/core';
import { CommonModule } from '@angular/common';

@Component({
  selector: 'app-input-button-unit',
  standalone: true,
  imports: [CommonModule],
  templateUrl: './input-button-unit.component.html',
  styleUrls: ['./input-button-unit.component.scss']
})
export class InputButtonUnitComponent {
  title = 'Hello World';

  changeTitle(newTitle: string): vois {
    this.title = newTitle;
  }
  
  getInputValue(event: Event): string {
    return (event.target as HTMLInputElement).value;
  }
}
```
{% endcode %}

{% code title="src/app/input-button-unit/input-button-unit.component.html" %}
```html
    <p>
      input-button-unit works!
      The title is: {{ title }}
    </p>

    <input [value]="title"
           (keyup.enter)="changeTitle(getInputValue($event))">

    <button (click)="changeTitle('Button Clicked!')">
      Save
    </button>
```
{% endcode %}

{% hint style="info" %}
💾 **Pusher votre code sur GitHub**

Committez tous vos changements en exécutant cette commande dans votre répertoire de projet.

```bash
git add -A && git commit -m "votre message de commit"
```

Puis pusher vos changements sur GitHub en exécutant cette commande dans votre répertoire de projet.

```
git push
```
{% endhint %}
