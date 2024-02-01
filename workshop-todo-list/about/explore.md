# b. Explorer

Tout comme nous l'avons fait dans le chapitre précédent, lorsque nous avons enregistré $event, vous pouvez faire de même avec `#inputElementRef`.


**Expérimentation:** Changez la méthode `changeTitle` pour qu'elle reçoive la référence de l'élément entier et l'affiche dans la console :

{% code title="src/app/input-button-unit/input-button-unit.component.ts" %}
```markup
<input #inputElementRef
       [value]="title"              
       (keyup.enter)="changeTitle(inputElementRef)">

<button (click)="changeTitle(inputElementRef)">
  Save
</button>
```
{% endcode %}

```typescript
changeTitle(inputElementReference) {
  console.log(inputElementReference);
  this.title = inputElementReference.value;
}
```

N'oubliez pas de remettre le code comme il était après avoir fini d'expérimenter ! Il est préférable de passer à une méthode exactement la valeur dont elle a besoin, au lieu de l'objet entier.

