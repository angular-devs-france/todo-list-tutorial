# #11: ⛓ Interface

Nous voulons utiliser les capacités de TypeScript pour savoir quel type d'objet nous passons en tant qu'`item` au composant `todo-item`. Nous nous assurerons que l'élément est du bon type. Mais son type n'est pas une simple string, nombre ou booléen. Nous allons donc définir le type de l'élément en utilisant une **interface**.


> Les interfaces n'existent que dans TypeScript et sont supprimées lorsque le code est transpilé en JavaScript. En JavaScript, les types ne sont pas vérifiés.

Créez une interface `TodoItem` dans un nouveau dossier `interfaces` avec _Angular CLI_:

```bash
ng g i interfaces/todo-item
```

`ì` est le raccourci de... vous l'avez deviné - interface. En ajoutant un chemin dans la commande, _Angular CLI_ génère les dossiers spécifiés s'ils n'existent pas déjà.

Dans votre IDE, ouvrez le fichier nouvellement créé `src/app/interfaces/todo-item.ts`:

{% code title="src/app/interfaces/todo-item.ts" %}
```typescript
export interface TodoItem {
}
```
{% endcode %}

Maintenant, nous pouvons définir quelles propriétés et/ou méthodes chaque objet de type TodoItem doit avoir. À ce stade, nous allons ajouter deux membres:

* `title` qui doit être de type `string`
* `completed` qui doit être de type `boolean` et facultatif

{% code title="src/app/interfaces/todo-item.ts" %}
```typescript
export interface TodoItem {
  title: string;
  completed?: boolean;
}
```
{% endcode %}

Definissons l'`@Input` de l'élément pour qu'il soit de ce nouveau type. Cela permettra à l'IDE de nous suggérer les membres disponibles lorsque nous utilisons l'élément dans le composant.

{% code title="src/app/todo-item/todo-item.component.ts" %}
```typescript
export class TodoItemComponent {
  @Input() item!: TodoItem;
```
{% endcode %}

Nous devons importer l'interface dans ce fichier.

{% code title="src/app/todo-item/todo-item.component.ts" %}
```typescript
import { TodoItem } from '../interfaces/todo-item';
```
{% endcode %}

Maintenant, définissons la liste des éléments pour contenir des objets du type `TodoItem`.

{% code title="src/app/app.component.ts" %}
```typescript
export class AppComponent {
  title = 'todo-list';
  todoList: TodoItem[] = [
    {title: 'install NodeJS'},
    {title: 'install Angular CLI'},
    {title: 'create new app'},
    {title: 'serve app'},
    {title: 'develop app'},
    {title: 'deploy app'},
  ];
}
```
{% endcode %}

Encore une fois, vous devez importer l'interface dans ce fichier.

{% code title="src/app/app.component.ts" %}
```typescript
import { TodoItem } from './interfaces/todo-item';
```
{% endcode %}

Maintenant essayez de supprimer le titre de l'un des objets de la liste. Que se passe-t-il?

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
