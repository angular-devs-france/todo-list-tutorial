# #8: 📎 Element ref - \#

Dans le dernier chapitre, nous avons terminé avec notre composant `InputButtonUnitComponent` capable d'afficher et de changer le titre de notre élément todo. `input-button-unit.component.ts` devrait ressembler à ceci :

{% code title="src/app/input-button-unit/input-button-unit.component.ts" %}
```typescript
import { Component, OnInit } from '@angular/core';

@Component({
  selector: 'app-input-button-unit',
  template: `
    <p>
      input-button-unit works!
      The title is: {{ title }}
    </p>

    <input [value]="title"
           (keyup.enter)="changeTitle(getInputValue($event))">

    <button (click)="changeTitle('Button Clicked!')">
      Save
    </button>
  `,  
  styleUrls: ['./input-button-unit.component.scss']  
})    
export class InputButtonUnitComponent implements OnInit {
  title = 'Hello World';

  constructor() { }                     

  ngOnInit(): void {
  }

  changeTitle(newTitle: string): void {
    this.title = newTitle;
  }
  
  
  getInputValue(event: Event) {
    return (event.target as HTMLInputElement).value;
  }
}
```
{% endcode %}

Tout d'abord, supprimons un peu du template html dont nous n'avons pas besoin. Supprimez ces lignes :

{% code title="remove this from src/app/input-button-unit/input-button-unit.component.ts" %}
```markup
<p>
  input-button-unit works!
  The title is: {{ title }}
</p>
```
{% endcode %}

Maintenant nous voulons prendre la valeur du champ \(que l'utilisateur a tapé\) et changer le titre quand nous appuyons sur le bouton `Save`.

Nous savons déjà comment créer un bouton et réagir au clic. Nous devons maintenant passer des données d'un élément différent à la méthode. Nous voulons utiliser la valeur de l'élément `input` à l'intérieur de l'élément `button`.

Angular nous aide à faire exactement cela. **Nous pouvons stocker une référence à l'élément que nous voulons dans une variable avec le nom que nous choisissons,** par exemple `inputElementRef`, **en utilisant une syntaxe simple - un `#`.** Ajoutez `#inputElementRef` à l'élément `input`, et utilisez-le dans l'événement `click` du bouton :

{% code title="src/app/input-button-unit/input-button-unit.component.ts" %}
```markup
template: `
  <input #inputElementRef
         [value]="title"
         (keyup.enter)="changeTitle($event.target.value)">

  <button (click)="changeTitle(inputElementRef.value)">
    Save
  </button>
`,
```
{% endcode %}

Maintenant nous pouvons utiliser la valeur que l'utilisateur a entrée dans l'élément `input` directement dans l'appel de méthode pour gérer le clic sur le bouton `Save` !

## What is that `#` we see?
## Qu'est-ce que ce `#` que nous voyons ?

`#` est un sucre syntaxique pour quelque chose appelé **variable de référence**. Angular nous permet de définir une nouvelle variable locale nommée `inputElementRef` \(ou tout autre nom que vous choisissez\) qui contient une référence à l'élément sur lequel nous l'avons définie, et ensuite de l'utiliser comme nous le souhaitons. Dans notre cas, nous l'utilisons pour accéder à la propriété `value` de l'`input`.

Plutôt que de partir à la chasse aux éléments en interrogeant le DOM directement (avec `document.getElementById`par exemple) \(ce qui est une mauvaise pratique, comme nous l'avons discuté\), nous pouvons maintenant mettre des références d'éléments dans le template et accéder à chaque élément que nous voulons de manière déclarative.

Ensuite, nous allons construire la liste des éléments de notre todo list.


💾 **Pusher votre code sur GitHub**

Committez tous vos changements en exécutant cette commande dans votre répertoire de projet.

```bash
git add -A && git commit -m "votre message de commit"
```

Pusher vos changements sur GitHub en exécutant cette commande dans votre répertoire de projet.

```
git push
```
{% endhint %}
