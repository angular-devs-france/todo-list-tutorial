# #17: 💾Local storage

Nous aimerions persister la liste de tâches sur notre ordinateur, afin que lorsque nous accédons ou rechargeons l'application, nous voyons la liste avec les modifications que nous avons apportées. Idéalement, la liste serait enregistrée dans une base de données, mais nous mettrons en œuvre une version simple à l'aide du stockage du navigateur.

## Qu'est-ce que le local storage ?

Local storage, comme son nom l'indique, est un outil pour stocker des données localement. Tout comme les cookies, le local storage stocke les données sur l'ordinateur de l'utilisateur et nous donne, en tant que développeurs, un moyen rapide d'accéder à ces données à la fois en lecture et en écriture.

## Découvrons l'API du local storage

Tout d'abord, pour utiliser le local storage, nous pouvons simplement accéder à une instance `localStorage` qui nous est exposée globalement. Cela signifie que nous pouvons appeler toutes les méthodes disponibles dans cette interface en utilisant simplement cette instance.

Le local storage enregistre les données sous forme de clé/valeur. Il a deux méthodes principales : `getItem` et `setItem`. Voici un exemple de leur utilisation :

{% code title="code for example" %}
```typescript
localStorage.setItem('name', 'Angular');

let name = localStorage.getItem('name'); 
alert(`Hello ${ name }!`);
```
{% endcode %}

Une autre méthode utile est `clear`. Elle est utilisée pour effacer toutes les données du local storage :

{% code title="code for example" %}
```typescript
localStorage.clear();
```
{% endcode %}

Il existe d'autres méthodes que vous pouvez utiliser plus occasionnellement, comme décrit dans la [documentation MDN Web](https://developer.mozilla.org/en-US/docs/Web/API/Storage).

## Implémentation dans notre application Angular

Dans la section suivante, nous allons construire un service de local storage qui sera utilisé pour stocker les éléments de notre liste de tâches. Ce sera un service générique pour les listes d'objets. Nous devrons lui indiquer le nom des données que nous recherchons (une clé), afin de pouvoir l'utiliser pour stocker d'autres listes également.

Comme pour les chapitres précédents, nous allons générer le service à l'aide de _Angular CLI_. Nous nommerons ce nouveau service `storage`.

```bash
ng g s services/storage
```

Le nouveau fichier `storage.service.ts` doit ressembler à ça :

{% code title="src/app/services/storage.service.ts" %}
```typescript
import { Injectable } from '@angular/core';

@Injectable({
  providedIn: 'root'
})
export class StorageService {

  constructor() { }

}
```
{% endcode %}

Si quelque chose vous semble inhabituel ou confus, veuillez vous référer au chapitre [Créer un service](creating-a-service.md) pour plus d'informations détaillées sur les services.

Puisque nous ne pouvons pas accéder directement à un élément de la liste dans le local storage, nous n'implémenterons que deux méthodes : obtenir les données et définir les données. Le changement de la liste sera effectué par le TodoListService. Pour chaque méthode, nous passerons la clé (nom) des données que nous voulons.

### getData

Cette méthode obtiendra et renverra les données (objet, liste, etc.) stockées dans le service sous la clé donnée :

{% code title="src/app/services/storage.service.ts" %}
```typescript
  getData(key: string): any {
    return JSON.parse(localStorage.getItem(key));
  }
```
{% endcode %}

Pourquoi utiliser `JSON.parse` ? La réponse est simple : comme décrit ci-dessus, le local storage stocke les données sous forme de paires clé-valeur, et les valeurs sont stockées sous forme de **string**. Donc, si nous voulons avoir un vrai objet (ou une liste) avec lequel travailler, nous devons analyser la string en un objet JavaScript valide.

### setData

Cette méthode enregistrera les données données (objet, liste, etc.) sous la clé donnée.

{% code title="src/app/services/storage.service.ts" %}
```typescript
  setData(key: string, data: any) {
    localStorage.setItem(key, JSON.stringify(data));
  }
```
{% endcode %}

Utilisons ce service dans notre `ToDoListService`.

> Comme mentionné ci-dessus, ce service pourrait avoir une API plus large avec des méthodes plus robustes. Lorsque vous écrivez un service pour accéder à une base de données, vous aurez d'autres méthodes pour ajouter, modifier et supprimer des éléments spécifiques.

## Utiliser StorageService

Nous aimerions utiliser notre nouveau service dans le `TodoListService`. Tout d'abord, nous injecterons `StorageService` dans `TodoListService`, tout comme nous avons injecté ce dernier dans `ListManagerComponent`. Nous demanderons une instance du service dans le constructeur, et nous assurerons que sa classe est importée. Nous déplacerons la liste de tâches par défaut en dehors de la classe. Nous ajouterons également une constante avec la clé de notre stockage.

{% code title="src/app/services/todo-list.service.ts" %}
```typescript
import { Injectable } from '@angular/core';
import { TodoItem } from '../interfaces/todo-item';
import { StorageService } from './storage.service';

const todoListStorageKey = 'Todo_List';

const defaultTodoList: TodoItem[] = [
  {title: 'install NodeJS'},
  {title: 'install Angular CLI'},
  {title: 'create new app'},
  {title: 'serve app'},
  {title: 'develop app'},
  {title: 'deploy app'},
];

@Injectable({
  providedIn: 'root'
})
export class TodoListService {
  todoList: TodoItem[];

  constructor(private storageService: StorageService) { }
}
```
{% endcode %}

Nous garderons une version de la liste de tâches en mémoire dans le service pour nous aider à la gérer dans l'application - la propriété `todoList`. Nous l'initialiserons dans le constructeur avec la liste dans le local storage, si elle existe, sinon la liste par défaut.

{% code title="src/app/services/todo-list.service.ts" %}
```typescript
constructor(private storageService: StorageService) {
  this.todoList = 
    storageService.getData(todoListStorageKey) || defaultTodoList;
}
```
{% endcode %}

Maintenant nous allons implémenter les méthodes pour gérer notre liste.

### addItem

Lorsque nous poussons un élément dans la todoList, nous allons ensuite mettre à jour le local storage.

{% code title="src/app/services/todo-list.service.ts" %}
```typescript
addItem(item: TodoItem): void {
  this.todoList.push(item);
  this.storageService.setData(todoListStorageKey, this.todoList);
}
```
{% endcode %}

### updateItem

Ici nous voulons mettre à jour un élément existant. Nous supposerons que nous détenons l'élément d'origine par référence, et que nous pouvons le trouver dans la liste. (D'autres implémentations peuvent utiliser un ID d'élément pour rechercher la liste.) Ensuite, nous le remplacerons par une nouvelle version. Enfin, nous mettrons à jour le local storage.

{% code title="src/app/services/todo-list.service.ts" %}
```typescript
updateItem(item: TodoItem, changes): void {
  const index = this.todoList.indexOf(item);
  this.todoList[index] = { ...item, ...changes };
  this.storageService.setData(todoListStorageKey, this.todoList);
}
```
{% endcode %}

Que ce passe-t-il ici ?
Nous localisons l'élément dans la liste. Puis au même endroit, nous assignons un nouvel objet, qui est construit à partir de l'élément d'origine et des modifications qui lui ont été apportées. Nous utilisons l'opérateur de propagation pour cela : un nouvel objet est construit, composé de l'ensemble original de clés et de valeurs (`...item`) qui sont remplacées par les clés et valeurs de `changes`. (Si une clé dans `changes` n'existe pas dans `item`, elle est ajoutée au nouvel objet.)

### DRY - Don't Repeat Yourself

Vous avez peut-être remarqué que nous avons la même ligne de code à la fois dans `addItem` et dans `updateItem` :

{% code title="src/app/services/todo-list.service.ts" %}
```typescript
this.storageService.setData(todoListStorageKey, this.todoList);
```
{% endcode %}

Nous aimerions réduire la répétition du code et extraire le code répété dans une méthode. Vous pouvez utiliser l'IDE pour vous aider à extraire la méthode. Sélectionnez la ligne ci-dessus, puis avec le click droit de la souris recherchez l'option pour refactoriser en extrayant une méthode. La méthode extraite devrait ressembler à ceci :

{% code title="src/app/services/todo-list.service.ts" %}
```typescript
saveList(): void {
    this.storageService.setData(todoListStorageKey, this.todoList);
}
```
{% endcode %}

Maintenant, assurez-vous d'appeler `saveList` dans les méthodes `addItem` et `updateItem`.

### deleteItem

Cette méthode supprimera un élément de la liste. Nous recherchons l'élément dans la liste, le supprimons et enregistrons les modifications.

{% code title="src/app/services/todo-list.service.ts" %}
```typescript
deleteItem(item: TodoItem): void {
  const index = this.todoList.indexOf(item);
  this.todoList.splice(index, 1);
  this.saveList();
}
```
{% endcode %}

`splice(i, n)` supprime `n` éléments à partir de l'index `i`. Dans notre code, nous ne voulons supprimer qu'un seul élément (c'est pourquoi nous utilisons 1 comme deuxième paramètre).

### Résultat final

Notre TodoListService est prêt avec des méthodes pour obtenir et modifier la liste de tâches. Nous pouvons utiliser ces méthodes à partir des composants.

{% code title="src/app/services/todo-list.service.ts" %}
```typescript
import { Injectable } from '@angular/core';
import { TodoItem } from '../interfaces/todo-item';
import { StorageService } from './storage.service';

const todoListStorageKey = 'Todo_List';

const defaultTodoList = [
  {title: 'install NodeJS'},
  {title: 'install Angular CLI'},
  {title: 'create new app'},
  {title: 'serve app'},
  {title: 'develop app'},
  {title: 'deploy app'},
];

@Injectable()
export class TodoListService {
  todoList: TodoItem[];

  constructor(private storageService: StorageService) {
    this.todoList = 
      storageService.getData(todoListStorageKey) || defaultTodoList;
  }

  saveList(): void {
    this.storageService.setData(todoListStorageKey, this.todoList);
}

  addItem(item: TodoItem): void {
    this.todoList.push(item);
    this.saveList();
  }

  updateItem(item: TodoItem, changes): void {
    const index = this.todoList.indexOf(item);
    this.todoList[index] = { ...item, ...changes };
    this.saveList();
  }

  deleteItem(item: TodoItem): void {
    const index = this.todoList.indexOf(item);
    this.todoList.splice(index, 1);
    this.saveList();
  }

  getTodoList(): TodoItem[] {
    return this.todoList;
  }

}
```
{% endcode %}

## Résumé

Dans ce chapitre, nous avons appris ce qu'est le local storage et comment l'utiliser. Nous avons vu que `localStorage` est un outil formidable et assez simple pour les développeurs pour stocker des données localement sur les ordinateurs/appareils des utilisateurs. Mais attention, les données ne doivent pas être sensibles.
Nous avons ensuite implémenté un nouveau service qui utilise ce `localStorage` pour stocker des données, que notre `TodoListService` utilise pour enregistrer les éléments de la liste de tâches.

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

