### Définir un type
Préférez l'union de type plutôt qu'une enumeration, moins lisible.
```
type taille = 'grand' | 'moyen' | 'petit';
const taille: visiteur = 'énorme';  // error
```   

## Directives structurelles
### @If
Détermine la création d'un élément du DOM à une condition.

L'alias est une astuce pour supporter la nullabilité des signaux. \
Il garantit que la valeur est *toujours* définie dans la structure conditionnelle.
```
@if (designation().libelle; as libel) {
  <h2 [textContent]="libel"></h2>
}
```

### @For
Permet d'effectuer des itérations pour parcourir une collection. Requiert une variable pour des besoins de performance (preferez l'identifiant s'il existe). \
L'expression `track` détermine une clé à associer aux *item* de la collection pour les vues du DOM.
```
@for (animal for animaux; track animal.id ) {
  <li [textContent]="animal.name"></li>
}
```

### Cycle de vie d'un composant
Les méthodes déterminant le comportement d'un composant durant son cycle de vie requièrent l'implémentation d'une interface dans le type (*viz.* la classe) ainsi qu'un import.
```
import { OnInit, AfterContentInit, AfterContentChecked, AfterViewInit, AfterViewChecked, OnDestroy } from '@angular/core';
```
```
export class Baleine implements OnInit, AfterContentInit, AfterContentChecked, AfterViewInit, AfterViewChecked, OnDestroy {

  public ngOnInit(): void {}
  public ngAfterContentInit(): void {}
  public ngAfterContentChecked(): void {}
  public ngAfterViewInit(): void {}
  public ngAfterViewChecked(): void {}
  public ngOnDestroy(): void {}
}
```
