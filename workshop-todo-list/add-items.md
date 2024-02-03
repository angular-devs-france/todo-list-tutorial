# #12: 📌Ajouter des éléments

Nous voulons ajouter des éléments à notre liste. Avec Angular, nous pouvons le faire facilement et voir l'élément ajouté immédiatement. Nous le ferons à partir du composant `input-button-unit` que nous avons créé précédemment. Nous allons le modifier pour que lorsque vous appuyez sur la touche Entrée ou cliquez sur le bouton Enregistrer, le nouvel élément sera ajouté à la liste avec comme titre la valeur de la zone de saisie.

Mais nous ne voulons pas que le composant `input-button-unit` soit responsable de l'ajout d'un nouvel élément à la liste. Nous voulons qu'il ait une responsabilité minimale, et **déléguer l'action à son composant parent**. L'un des avantages de cette approche est que ce composant sera réutilisable, et peut être attaché à une action différente dans différentes situations.

Par exemple, dans notre cas, nous pourrons utiliser le composant `input-button-unit` à l'intérieur du composant `todo-item`. Nous aurons alors une zone de saisie pour chaque élément et nous pourrons modifier le titre de l'élément. Dans ce cas, appuyer sur la touche Entrée ou sur le bouton Enregistrer aura un effet différent.

Donc ce que nous voulons faire, c'est **émettre un événement** à partir du composant `input-button-unit` chaque fois que le titre est modifié. Avec Angular, nous pouvons facilement définir et émettre des événements à partir de nos composants !

## @Output()

Ajoutez la ligne suivante à l'intérieur de la classe `InputButtonUnitComponent`, qui définit une sortie pour le composant :

{% code title="src/app/input-button-unit/input-button-unit.component.ts" %}
```typescript
@Output() submit: EventEmitter<string> = new EventEmitter<string>();
```
{% endcode %}

La propriété `@Output` s'appelle `submit`. Elle est de type `EventEmitter` qui a une méthode `emit`. `EventEmitter` est un type générique - nous lui passons un autre type qui sera utilisé en interne, dans ce cas c'est `string`. C'est le type de l'objet qui sera émis par la méthode `emit`.

Assurez-vous que `Output` et `EventEmitter` sont bien importés dans la première ligne du fichier :

{% code title="src/app/input-button-unit.component.ts" %}
```typescript
import { Component, Output, EventEmitter} from '@angular/core';
```
{% endcode %}

Maintenant, chaque fois que nous appelons `this.submit.emit()`, un événement sera émis vers le composant parent. Appelons-le dans la méthode `changeTitle` :

{% code title="src/app/input-button-unit/input-button-unit.component.ts" %}
```typescript
changeTitle(newTitle: string): void {
  this.submit.emit(newTitle);
}
```
{% endcode %}

Nous déléguons tout au composant parent - même le changement du titre de l'élément si nécessaire.

Nous passons `newTitle` lorsque nous émettons l'événement. Tout ce que nous passons dans `emit()` sera disponible pour le parent en tant que `$event`. Les événements émis par `keyup.enter` et `click` appellent toujours la même méthode, mais la méthode elle-même a changé.

Le nom de la méthode ne correspond plus à l'action qu'elle fournit. En effet, elle ne change plus le titre. Changeons-le pour quelque chose de plus approprié : `submitValue`. Vous pouvez utiliser les outils de l'IDE pour renommer la méthode - assurez-vous qu'elle est également modifiée dans le modèle.

{% code title="src/app/input-button-unit/input-button-unit.component.ts" %}
```typescript
submitValue(newTitle: string): void {
  this.submit.emit(newTitle);
}
```
{% endcode %}

{% code title="src/app/input-button-unit/input-button-unit.component.html" %}
```html
  <input #inputElementRef
         [value]="title"
         (keyup.enter)="submitValue(getInputValue($event))">

  <button (click)="submitValue(inputElementRef.value)">
    Save
  </button>
```
{% endcode %}

### Ecoutons l'événement

Maintenant nous devons simplement attraper l'événement dans le composant parent et y attacher la logique. Allez dans le composant `app-root` et liez l'événement `submit` dans le composant `<app-input-button-unit>` :

{% code title="src/app/app.component.html" %}
```markup
<app-input-button-unit (submit)="addItem($event)"></app-input-button-unit>
```
{% endcode %}

Maintenant, il ne reste plus qu'à implémenter la méthode `addItem`. Elle reçoit une string qui contient le titre. Elle doit créer un objet avec la propriété `title` et enfin ajouter cet object à la liste :

{% code title="src/app/app.component.ts" %}
```typescript
addItem(title: string): void {    
  this.todoList.push({ title: title });
}
```
{% endcode %}

Essayez - entrez un nouveau titre de tâches dans le champ de saisie et soumettez-le !

**Note:** Nous pouvons utilisons **ES6 Object Property Value Shorthand** pour construire l'objet élément à faire. Si nous utilisons une variable avec le même nom que la propriété de l'objet à laquelle nous voulons assigner la valeur de la variable, nous pouvons utiliser cette notation abrégée. Dans notre cas, `{ title: title }` est équivalent à `{ title }`. Si la chaîne était stockée dans une variable avec un nom différent, nous ne pouvons pas pu utiliser la notation abrégée. Par exemple:

{% code title="code for example" %}
```typescript
addItem(value: string): void {    
  this.todoList.push({ title: value });
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
