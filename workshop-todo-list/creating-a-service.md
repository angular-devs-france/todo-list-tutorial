# #15: 🔋Créer un service

Dans Angular, un service est une classe JavaScript qui est responsable d'effectuer une tâche spécifique nécessaire à votre application. Dans notre application todo-list, nous allons créer un service qui sera responsable de l'enregistrement et de la gestion de toutes les tâches, et nous l'utiliserons en l'injectant dans les composants.

## Créer un service avec Angular CLI:

```
ng g s services/todo-list
```
`s` est le raccourci de... vous l'avez deviné - service.
Cette commande va générer le service `TodoListService` dans le fichier `src/app/services/todo-list.service.ts`. Il a le décorateur `@Injectable` qui lui permet d'utiliser l'injection de dépendance.

{% code title="src/app/services/todo-list.service.ts" %}
```
import { Injectable } from '@angular/core';

@Injectable({
  providedIn: 'root'
})
export class TodoListService {

  constructor() { }
}
```
{% endcode %}

## Partager des données

Maintenant nous pouvons déplacer le tableau `todoList` du composant `list-manager` vers notre nouveau service. 
Allez dans le fichier `src/app/services/todo-list.service.ts`, et ajoutez ce code à l'intérieur de la classe `TodoListService` juste au-dessus du `constructor` :

{% code title="src/app/services/todo-list.service.ts" %}
```typescript
private todoList: TodoItem[] = [
  {title: 'install NodeJS'},
  {title: 'install Angular CLI'},
  {title: 'create new app'},
  {title: 'serve app'},
  {title: 'develop app'},
  {title: 'deploy app'},
];
```
{% endcode %}

Assurez-vous que l'interface TodoItem est bien importée :

{% code title="src/app/services/todo-list.service.ts" %}
```typescript
import { TodoItem } from '../interfaces/todo-item';
```
{% endcode %}

## Créer une méthode pour retourner la liste

Nous allons ajouter une méthode `getTodoList` qui va retourner le tableau `todoList`. Le service ressemblera à ceci:

{% code title="src/app/services/todo-list.service.ts" %}
```typescript
import { Injectable } from '@angular/core';
import { TodoItem } from '../interfaces/todo-item';

@Injectable({
  providedIn: 'root'
})
export class TodoListService {

  private todoList: TodoItem[] = [
    {title: 'install NodeJS'},
    {title: 'install Angular CLI'},
    {title: 'create new app'},
    {title: 'serve app'},
    {title: 'develop app'},
    {title: 'deploy app'},
  ];

  constructor() { }

  getTodoList(): TodoItem[] {
    return this.todoList;
  }
}
```
{% endcode %}

## Injecter et utiliser le service

Après avoir créé le service, nous pouvons l'injecter dans notre composant `list-manager`. Dans Angular, l'injection de dépendance est très simple. Nous le passons en paramètre dans le constructeur - le type de paramètre est le nom de la classe du service. Angular attribue l'instance qu'il a créée au nom du paramètre, et nous pouvons l'utiliser à partir du constructeur. Avant de l'implémenter nous-mêmes, voyons comment cela fonctionne. Voici le constructeur de la classe `ListManagerComponent`:

{% code title="src/app/list-manager/list-manager.component.ts" %}
```typescript
constructor(todoListService: TodoListService) {
  todoListService.getTodoList();
}
```
{% endcode %}

Typescript nous aide en nous donnant un raccourci pour assigner le paramètre à un membre de classe. En ajoutant `private` ou `public` avant le nom du paramètre, il est automatiquement assigné à `this`. Donc au lieu de déclarer et d'assigner la propriété par nous-mêmes :

```typescript
export class ListManagerComponent {
  todoListService: TodoListService;

  constructor(todoListService: TodoListService) { 
    this.todoListService = todoListService;
  }
}
```

... nous pouvons réduire beaucoup de code comme ceci:

```typescript
export class ListManagerComponent {

  constructor(private todoListService: TodoListService) { }
}
```

Utilisons le service dans le composant `list-manager`.

* Supprimez la liste codée en dur du composant, ne gardez que la déclaration de la propriété `todoList` avec son type.
* Injectez le `TodoListService` par le constructeur.

{% code title="src/app/list-manager/list-manager.component.ts" %}

```typescript
export class ListManagerComponent {
  todoList: TodoItem[];

  constructor(private todoListService: TodoListService) { }
```

* Assurez-vous que le `TodoListService` est bien importé.

{% code title="src/app/list-manager/list-manager.component.ts" %}
```typescript
import { TodoListService } from '../services/todo-list.service';
```
{% endcode %}

* Récupérez la liste du service dans le `constructor` et l'assigner à notre propriété.

{% code title="src/app/list-manager/list-manager.component.ts" %}
```typescript
constructor(private todoListService: TodoListService) {
  this.todoList = this.todoListService.getTodoList();
}
```
{% endcode %}

Vous n'avez pas besoin de changer quoi que ce soit dans le modèle puisque nous assignons la liste à la même propriété que nous avons utilisée auparavant. Il semble que rien n'ait changé, mais vous pouvez vérifier que la liste provient du service en la changeant à partir de là (ajout d'un élément, changement de titre, etc.).

Si la liste n'est pas affichée et qu'aucune erreur ne se produit lors de la compilation, le projet pourrait ne pas être synchronisé avec l'ajout du fichier de service. Arrêtez l'exécution de `ng serve` dans le terminal en cliquant sur `Ctrl+C` et exécutez-le à nouveau.

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
