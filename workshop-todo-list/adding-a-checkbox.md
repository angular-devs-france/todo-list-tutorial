# #19: 🔘 Ajout d'une checkbox

Nous sommes maintenant capables d'interagir avec notre liste de tâches en supprimant des éléments. Mais que se passe-t-il si nous voulons check des éléments mais être toujours capable de les voir dans notre liste avec le titre de l'élément barré ? Une checkbox !

Dans cette section, nous allons :

* Ajouter une checkbox
* Ajouter une fonctionnalité lorsque vous cliquez sur la checkbox afin qu'une classe CSS, qui ajoute un style ~~strikethrough~~ à nos éléments terminés
* Ajouter une fonctionnalité pour enregistrer l'état de l'élément dans le local storage
* Ajouter une nouvelle classe CSS

Ajoutons une checkbox dans notre fichier `todo-item.component.ts`. Placez le code suivant juste avant `{{ item.title }}` :

{% code title="src/app/todo-item/todo-item.component.html" %}
```markup
<input type="checkbox"/>
```
{% endcode %}

Maintenant pour que la checkbox fasse quelque chose, nous devons ajouter un gestionnaire d'événements `click` que nous appellerons `completeItem`. pour le style, nous allons ajouter une classe CSS à notre checkbox puis envelopper l'élément et l'interpolation ensemble dans une `div`. Faisons-le maintenant :

{% code title="src/app/todo-item/todo-item.component.html" %}
```markup
<div>
  <input type="checkbox"
         class="todo-checkbox"
         (click)="completeItem()"/>
  {{ item.title }}
</div>
```
{% endcode %}

Quand nous cliquons sur la checkbox, elle exécute la méthode `completeItem`. Parlons de ce que cette méthode doit accomplir. Nous voulons être capable de basculer un style CSS sur le titre de l'élément de sorte que lorsque la checkbox est cochée, il aura un ~~strikethrough~~. Nous voulons aussi enregistrer l'état de l'élément dans le local storage. Pour ce faire, nous émettrons un événement de mise à jour avec le nouveau statut de l'élément et le capturerons dans le composant parent.

{% code title="src/app/todo-item/todo-item.component.ts" %}
```javascript
export class TodoItemComponent {
  @Input() item: TodoItem;
  @Output() remove: EventEmitter<TodoItem> = new EventEmitter<TodoItem>();
  @Output() update: EventEmitter<any> = new EventEmitter<any>();

  completeItem(): void {
    this.update.emit({
      item: this.item,
      changes: { completed: !this.item.completed }
    });
  }
```
{% endcode %}

Afin que la checkbox reflète le statut terminé, nous devons ajouter une liaison de propriété pour son statut comme ceci :

{% code title="src/app/todo-item/todo-item.component.html" %}
```html
<div>
  <input type="checkbox"
         class="todo-checkbox"
         (click)="completeItem()"
         [checked]="item.completed"/>
  {{ item.title }}
</div>
```
{% endcode %}

Attendez! Comment est-ce que tout cela va affecter le titre de la todo quand nous ne touchons qu'à la checkbox? Eh bien, Angular a cette merveilleuse directive appelée NgClass. Cette directive applique ou supprime une classe CSS en fonction d'un booléen. Il existe de nombreuses façons d'utiliser cette directive (voir la documentation de la directive [NgClass](https://angular.io/api/common/NgClass)) mais nous nous concentrerons sur son utilisation comme ceci :

```markup
<some-element [ngClass]="{'first': true, 'second': true, 'third': false}">...</some-element>
```

Les classes 'first' et 'second' seront appliquées à l'élément parce qu'elles ont une valeur vraie, tandis que la classe 'third' ne sera pas appliquée parce qu'elle a une valeur fausse. C'est donc là que notre code précédent entre en jeu. Notre méthode `completeItem` basculera entre les valeurs vraies et fausses, dictant ainsi si une classe doit être appliquée ou non.

Plaçons le titre de l'élément dans un `<span>`, puis utilisons NgClass pour appliquer le style. En fonction du champ completed de l'élément actuel, nous affichons le titre barré ou non :

```markup
<span class="todo-title" [ngClass]="{'todo-complete': item.completed}">
  {{ item.title }}
</span>
```

Et enfin, ajoutez le CSS à notre fichier `todo-item.component.scss` :

```css
  .todo-complete {
    text-decoration: line-through;
  }
```

La prochaine étape consiste à dire à l'élément parent `list-manager` quoi faire, lorsque l'événement de mise à jour est émis. Pour ce faire, nous devons lier l'action de mise à jour et la méthode de mise à jour qui déclenchera une fonction appropriée dans `TodoListService`. Trouvez le sélecteur `app-todo-item` dans le modèle :

{% code title="src/app/list-manager/list-manager.component.html" %}
```html
<li>
  <app-todo-item [item]="todoItem" (remove)="removeItem($event)"></app-todo-item>
</li>
```
{% endcode %}

Et appliquez les modifications :

{% code title="src/app/list-manager/list-manager.component.html" %}
```html
<li>
  <app-todo-item [item]="todoItem"
     (remove)="removeItem($event)"
     (update)="updateItem($event.item, $event.changes)"></app-todo-item>
</li>
```
{% endcode %}

Enfin créer une méthode supplémentaire pour gérer cet événement de mise à jour de l'élément. Elle ressemblera beaucoup à la fonction `removeItem` :

{% code title="src/app/list-manager/list-manager.component.ts" %}
```typescript
updateItem(item: TodoItem, changes): void {
  this.todoListService.updateItem(item, changes);
}
```
{% endcode %}

Voilà! Cocher la checkbox doit appliquer un style barré au titre de l'élément, alors que décocher la checkbox doit supprimer le style barré du titre de l'élément.

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
