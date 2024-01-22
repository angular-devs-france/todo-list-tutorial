# #10: ➕ Nouveau composant: todo-item

Nous allons créer un nouveau composant pour afficher chaque élément de la liste de tâches. Ce sera un composant simple au début, mais il va grandir par la suite. Ce qui est important, c'est qu'il recevra l'élément de la liste de tâches par le composant parent. De cette façon, il peut être un composant réutilisable, et ne pas dépendre directement des données et de l'état de l'application.

Dans le terminal, créez un nouveau composant appelé `todo-item` avec la commande suivante qui utilise les raccourcies :

```
ng g c todo-item
```
`g` est un raccourci pour `generate`et `c` est un raccourci pour `component`

De retour dans votre IDE, vous pouvez voir qu'un nouveau dossier a été créé - `src/app/todo-item`, avec les fichiers du composant à l'intérieur.

Utilisez ce nouveau composant dans le modèle du composant `app-root` - à l'intérieur de l'élément `<li>`:

{% code title="src/app/app.component.html" %}
```markup
<ul>
  @for(let todoItem of todoList; track todoItem.title) {
    <li>
      <app-todo-item></app-todo-item>
    </li>
  }
</ul>
```
{% endcode %}

Verifiez le résultat dans le navigateur. Que voyez-vous? Pourquoi?

## @Input()

Nous voulons afficher le titre de chaque élément dans le composant `todo-item`. Nous devons donc passer l'élément actuel de la boucle au composant `todo-item`.

Angular nous facilite la tâche en nous fournissant le décorateur `Input`.

Dans la classe `TodoItemComponent` nouvellement générée dans `todo-item.component.ts`, ajoutez la ligne:

{% code title="src/app/todo-item/todo-item.component.ts" %}
```typescript
@Input() item;
```
{% endcode %}

Assurez-vous que `Input` est bien importé dans la première ligne du fichier. 
Cette ligne indique au composant de s'attendre à une entrée et de l'assigner à la propriété `item`. 
Maintenant, nous pouvons utiliser `item` à l'intérieur du modèle `todo-item` et extraire le titre de l'élément avec l'interpolation : `{{ item.title }}`.

Le composant devrait ressembler à ceci maintenant :

{% code title="src/app/todo-item/todo-item.component.ts" %}
```typescript
import { Component, Input, OnInit } from '@angular/core';

@Component({
  selector: 'app-todo-item',
  standalone: true,
  imports: [CommonModule],
  templateUrl: './todo-item.component.html',
  styleUrls: ['./todo-item.component.scss']
})
export class TodoItemComponent {
  @Input() item;
}
```
{% endcode %}

{% code title="src/app/todo-item/todo-item.component.html" %}
```html
 {{ item.title }}

```
{% endcode %}

Maintenant nous devons donner au composant `app-todo-item` l'élément qu'in attend avec l'`Input`. Retournez au composant `app-root` et passez l'élément au composant `app-todo-item`:

{% code title="src/app/app.component.html" %}
```markup
<ul>
  @for(let todoItem of todoList; track todoItem.title) {
    <li>
      <app-todo-item [item]="todoItem"></app-todo-item>
    </li>
  }
</ul>
```
{% endcode %}

Le `item` ici entre crochets est le même que celui déclaré comme `@Input` du composant.

Nous avons utilisé la liaison de propriété sur un élément que nous avons créé nous-mêmes ! Et maintenant, nous pouvons réellement voir et comprendre que la liaison de propriété se lie à une propriété réelle du composant. Bientôt, nous verrons comment cette liste peut être dynamique.

{% hint style="info" %}
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
