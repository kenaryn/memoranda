### Définir un type
Préférez l'union de type plutôt qu'une enumeration, moins lisible.
```
type taille = 'grand' | 'moyen' | 'petit';
const taille: visiteur = 'énorme';  // error
```   

## Cycle de détection de changement
### Les signaux
Il ne faut pas modifier directement l'objet signal. \
Privilégiez la création d'une copie mise à jour du signal:
```angular2html
protected readonly designation: WritableSignal<{ libelle: string }> = signal({ libelle: 'Formulaire de remplissage' });

public get libelle(): string {
return this.designation().libelle;
}

public set libelle(nouveauLibelle: string) {
this.designation.update(d => ({ ...d, libelle: nouveauLibelle }));
```
Traitez le signal comme une valeur immuable. Ici, la méthode `update()` remplace l'ancienne valeur par une nouvelle.

Ajoutez une logique de contrôle dans le composant:
```angular2html
public getLibelleSiNonVide(): string | null {
  const val = this.libelle;
  return val && (val.trim().length > 0) ? val : null;
}
```
et remplacez les invocations de signaux "hard-codées" dans le template par l'appel aux accesseurs et sa nouvelle méthode:
```angular2html
@if (getLibelleSiNonVide()) {
<h2 [textContent]="libelle"></h2>
}
<form>
  <fieldset>
    <legend [textContent]="libelle"></legend>
    <label for="libelle">Libellé : </label>
    <input type="text" id="libelle" name="libelle" [(ngModel)]="libelle" />
```

Dorénavant, le gabarit HTML (*viz.* le template) assure une liaison de données bi-directionnelle selon les bonnes pratiques d'Angular!


## Directives structurelles
### @If
Détermine la création d'un élément du DOM à une condition.

L'alias est une astuce pour supporter la nullabilité des signaux. \
Il garantit que la valeur est *toujours* définie dans la structure conditionnelle.
```angular2html
@if (designation().libelle; as libel) {
  <h2 [textContent]="libel"></h2>
}
```

Attention toutefois, car l'alias est assigné à la dernière expression évaluée qui détermine la vérité de la condition, *e.g.*:
```angular2html
@if (designation().libelle && (designation().libelle.trim().length > 0); as libel) {
  <h2 [textContent]="libel"></h2>
}
```
Ici, `libel` retourne donc `boolean | ''` et non la propriété `libelle` du signal `designation` comme attendu.

### @For
Permet d'effectuer des itérations pour parcourir une collection. Requiert une variable pour des besoins de performance (preferez l'identifiant s'il existe). \
L'expression `track` détermine une clé à associer aux *item* de la collection pour les vues du DOM.
```angular2html
@for (animal for animaux; track animal.id ) {
  <li [textContent]="animal.name"></li>
}
```

### Cycle de vie d'un composant
Les méthodes déterminant le comportement d'un composant durant son cycle de vie requièrent l'implémentation d'une interface dans le type (*viz.* la classe) ainsi qu'un import.
```angular2html
import { OnInit, AfterContentInit, AfterContentChecked, AfterViewInit, AfterViewChecked, OnDestroy } from '@angular/core';
```

```angular2html
export class Baleine implements OnInit, AfterContentInit, AfterContentChecked, AfterViewInit, AfterViewChecked, OnDestroy {

  ngOnInit(): void {}
  ngAfterContentInit(): void {}
  ngAfterContentChecked(): void {}
  ngAfterViewInit(): void {}
  ngAfterViewChecked(): void {}
  ngOnDestroy(): void {}
}
```

### Difference between Array and []
The `readonly` type modifier can only be applied to tuple-based syntax types. This is *not* a general-purpose type operator:
```angular2html
let err1: readonly Set<number>;  // error
let err2: readonly Array<boolean>; // error

let fine: readonly boolean[];  // works fine
```


### Politique d'accès
Pour rendre une méthode utilitaire d'un composant accessible à son template, utilisez un modificateur de type `protected`. \
En effet, la politique d'accès `private` est réservée à la classe elle-même.


### Objet litéral et propriétés calculées
Lorsque vous retournez un objet litéral depuis une méthode et insérer une paire de parenthèses pour effectuer un calcul, soit afin de garantir la préséance des opérateurs ou assurer la lisibilité dudit calcul, une erreur inattendue survient.

De là, le compilateur peut traiter la première valeur de l'objet retourné comme une méthode qu'il faut invoquer avec la computation suivante, analysée grammaticalement comme un tuple d'arguments. \
Bien sûr, la première propriété ne disposant pas de signature d'appel (ce n'est qu'une valeur), une erreur est levée.
Il vous est donc nécessaire de préfixer ce calcul par l'ajout d'une propriété calculée, soit un nouvel label décrivant le calcul et suivi du morphème `:`. *e.g.*:
```angular2html
return { distanceKmTotale, consommationMoyenne: (consommationPour100Km / this.voyages.length) };  
```

Ici, la propriété calculée `consommationMoyenne` est ajoutée pour permettre l'analyse grammaticale correcte de l'expression entre parenthèses suivante.
