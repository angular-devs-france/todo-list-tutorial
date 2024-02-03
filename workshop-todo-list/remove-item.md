# #18: 🗑 Supprimer un item

L'utilisateur doit pouvoir supprimer n'importe quel élément. La suppression d'un élément se fera en cliquant sur un bouton, nommé à juste titre "Remove". Dans ce chapitre, nous allons apprendre à ajouter cette fonctionnalité à notre projet.

## Ajouter un bouton 'Remove'

Tout d'abord, nous devons ajouter le bouton à l'élément, nous allons donc travailler sur le fichier `todo-item.component.html`.

Ajoutez un bouton "Remove" au modèle de l'élément, avec un gestionnaire d'événements `click` qui appelle une méthode `removeItem` (que nous allons créer dans un instant):

{% code title="src/app/todo-item/todo-item.component.html" %}
```html
  <div class="todo-item">
    {{ item.title }}

    <button class="btn btn-red" (click)="removeItem()">
      Remove
    </button>
  </div>
```
{% endcode %}

Ajoutez un nouvel `@Output` à la classe `TodoItemComponent`, qui émettra l'élément supprimé au gestionnaire de liste lorsque l'utilisateur appuie sur le bouton "Remove" :

{% code title="src/app/todo-item/todo-item.component.ts" %}
```typescript
@Output() remove: EventEmitter<TodoItem> = new EventEmitter<TodoItem>();
```
{% endcode %}

Assurez-vous d'importer à la fois `EventEmitter` et `Output` :

{% code title="src/app/todo-item/todo-item.component.ts" %}
```typescript
import { Component, Input, EventEmitter, Output } from '@angular/core';
```
{% endcode %}

Ajoutez une méthode à la classe `ItemComponent` pour émettre l'événement. Cette méthode sera appelée lorsque l'utilisateur clique sur le bouton "Remove" :

```typescript
removeItem(): void {
  this.remove.emit(this.item);
}
```

## Supprimez l'item de la liste

Maintenant que chaque item peut émettre sa propre suppression, assurons nous que le gestionnaire de liste supprime effectivement cet item de la liste. Pour cela, nous allons travailler sur le fichier `list-manager.component.ts`.

Nous devons répondre à l'événement `remove`. Ajoutons-le au modèle, à l'intérieur de la balise `<todo-item>` :

{% code title="src/app/list-manager/list-manager.component.html" %}
```markup
<app-todo-item [item]="todoItem"
               (remove)="removeItem($event)"></app-todo-item>
```
{% endcode %}

Maintenant nous devons simplement ajouter la méthode `removeItem()` à la classe `ListManagerComponent`, et utiliser la méthode `deleteItem` du service `todoListService` pour supprimer l'item de la liste et mettre à jour le local storage :

{% code title="src/app/list-manager/list-manager.component.ts" %}
```typescript
removeItem(item: TodoItem): void {
  this.todoListService.deleteItem(item);
}
```
{% endcode %}

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
