# Airbnb CSS / Sass Styleguide

*Une approche raisonnable de CSS et Sass*

## Table des matières

1. [Terminologie](#terminologie)
    - [Bloc de déclaration](#bloc-de-déclaration)
    - [Sélecteurs](#sélecteurs)
    - [Propriétés](#propriétés)
1. [CSS](#css)
    - [Mise en forme](#mise-en-forme)
    - [Commentaires](#commentaires)
    - [OOCSS et BEM](#oocss-et-bem)
    - [Sélecteurs d'ID](#sélecteurs-did)
    - [JavaScript hooks](#javascript-hooks)
    - [Bordure](#bordure)
1. [Sass](#sass)
    - [Syntaxe](#syntaxe)
    - [Ordre](#ordre-des-propriétés)
    - [Variables](#variables)
    - [Mixins](#mixins)
    - [Directive Extend](#directive-extend)
    - [Sélecteurs imbriqués](#sélecteurs-imbriqués)
1. [Traductions](#traductions)

## Terminologie

### Bloc de déclaration

Un « bloc de déclaration » est le nom donné à un sélecteur (ou un groupe de sélecteurs) accompagné d'un groupe de propriétés. Un exemple ici :

```css
.listing {
  font-size: 18px;
  line-height: 1.2;
}
```

### Sélecteurs

Dans un bloc de déclaration, les « sélecteurs » sont les parties qui déterminent quels éléments dans l'arborescence DOM seront stylés par les propriétés définies. Les sélecteurs peuvent correspondre à des éléments HTML, ainsi que la classe d'un élément, son ID ou l'un de ses attributs. Ci-après, quelques exemples de sélecteurs :

```css
.my-element-class {
  /* ... */
}

[aria-hidden] {
  /* ... */
}
```

### Propriétés

Enfin, les propriétés sont ce qui donne leur style aux éléments sélectionnés d'un bloc de déclaration. Les propriétés vont de paires avec leur valeur respective, et un bloc de déclaration peut contenir une ou plusieurs propriétés. Les propriétés ressemblent à ceci :

```css
/* some selector */ {
  background: #f1f1f1;
  color: #333;
}
```

**[⬆ Retour en haut](#table-des-matieres)**

## CSS

### Mise en forme

* Utiliser des « soft tabs » (2 espaces) pour l'indentation
* Préférer les tirets plutôt que le camelCase dans les noms de classes.
  - Les Underscores et le PascalCase sont acceptés si vous utilisez BEM (voir [OOCSS et BEM](#oocss-et-bem) ci-dessous).
* Ne pas utiliser les sélecteurs d'ID.
* Donner à chaque sélecteur sa propre ligne, lorsque vous utilisez plusieurs sélecteurs dans un bloc de déclaration.
* Mettre un espace avant l'accolade ouvrante `{` dans les blocs de déclaration.
* Dans les propriétés, mettre un espace après, mais pas avant, le caractère `:`.
* Mettre l'accolade fermante `}` à la ligne dans les blocs de déclaration.
* Mettre un retour de la ligne entre chaque bloc de déclaration.

**Mauvais**

```css
.avatar{
    border-radius:50%;
    border:2px solid white; }
.no, .nope, .not_good {
    // ...
}
#lol-no {
  // ...
}
```

**Bon**

```css
.avatar {
  border-radius: 50%;
  border: 2px solid white;
}

.one,
.selector,
.per-line {
  // ...
}
```

### Commentaires

* Préférer les commentaires de ligne (`//` dans Sass) pour commenter.
* Préférer les commentaires sur leur propre ligne. Eviter les commentaires en fin de ligne.
* Ecrire des commentaires détaillés pour le code qui n'est pas explicite :
  - L'utilisation des z-index
  - La compatibilité ou les hacks spécifique de navigateurs

### OOCSS et BEM

Nous encourageons la combinaison d'OOCSS et de BEM pour ces raisons :

  * Cela aide à créer des relations claires et strictes entre CSS et HTML
  * Cela nous aide à créer des composants réutilisables
  * Cela permet moins d'imbriquation et une faible spécificité
  * Cela aide à créer des feuilles de style évolutives

**OOCSS**, ou « Object Oriented CSS », est une approche d'écriture du CSS qui vous encourage à penser à votre feuille de style en tant que collection d'« objets » : du code réutilisable et répétitif qui peut être utilisé de manière autonome sur un site Web.

  * Nicole Sullivan's [OOCSS wiki](https://github.com/stubbornella/oocss/wiki)
  * Smashing Magazine's [Introduction to OOCSS](http://www.smashingmagazine.com/2011/12/12/an-introduction-to-object-oriented-css-oocss/)

**BEM**, ou « Block-Element-Modifier », est une _convention de nommage_ pour les classes en HTML et CSS. Il a été développé à l'origine par Yandex avec une grande base de code et une évolutivité à l'esprit, et peut servir de lignes directrices pour la mise en œuvre d'OOCSS.

  * CSS Trick's [BEM 101](https://css-tricks.com/bem-101/)
  * Harry Roberts' [introduction to BEM](http://csswizardry.com/2013/01/mindbemding-getting-your-head-round-bem-syntax/)

Nous recommandons une variante de BEM avec des « blocs » en PascalCase, qui fonctionne particulièrement bien quand ils sont combinés avec des composants (par exemple, React). Les underscores and tirets sont toujours utilisés pour les modificateurs et les descendants.

**Exemple**

```jsx
// ListingCard.jsx
function ListingCard() {
  return (
    <article class="ListingCard ListingCard--featured">

      <h1 class="ListingCard__title">Adorable 2BR in the sunny Mission</h1>

      <div class="ListingCard__content">
        <p>Vestibulum id ligula porta felis euismod semper.</p>
      </div>

    </article>
  );
}
```

```css
/* ListingCard.css */
.ListingCard { }
.ListingCard--featured { }
.ListingCard__title { }
.ListingCard__content { }
```

  * `.ListingCard` est un « bloc » et représente le plus haut niveau du composant
  * `.ListingCard__title` est un « élément » et repésente un descendant de `.ListingCard` qui aide à composer le bloc dans son ensemble.
  * `.ListingCard--featured` es un « modificateur » et représente un état différent ou une variation sur le bloc `.ListingCard`.

### Sélecteurs d'ID

Alors qu'il est possible de sélectionner un élément par son ID en CSS, il devrait généralement être considéré comme un « anti-pattern ». Les sélecteurs d'ID introduisent un niveau inutilement élevé de [spécificité](https://developer.mozilla.org/en-US/docs/Web/CSS/Specificity) à vos blocs de déclaration, et ne sont pas réutilisables.

Pour plus d'information sur ce sujet, lisez [CSS Wizardry's article](http://csswizardry.com/2014/07/hacks-for-dealing-with-specificity/) sur la spécificité.

### JavaScript hooks

Évitez de lier la même classe dans votre CSS et votre JavaScript. La combinaison des deux entraîne souvent, au minimum, un temps perdu lors du refactorisation lorsqu'un développeur doit faire une référence croisée sur chaque classe, et dans le pire des cas, les développeurs auront peur de faire des changements en cassant des fonctionnalités.

Nous vous recommandons de créer des classes spécifiques à JavaScript, préfixées avec `.js-` :

```html
<button class="btn btn-primary js-request-to-book">Request to Book</button>
```

### Bordure

Utiliser `0` à la place de `none` pour spécifier qu'un style n'a pas de bordure.

**Mauvais**

```css
.foo {
  border: none;
}
```

**Bon**

```css
.foo {
  border: 0;
}
```
**[⬆ Retour en haut](#table-des-matieres)**

## Sass

### Syntaxe

* Utiliser la syntaxe `.scss`, jamais la syntaxe originelle `.sass`.
* Ordonner le code CSS et `@include` logiquement (voir ci-dessous).

### Ordre des propriétés

1. Déclaration de propriétés

    Lister toutes les propriétés standard, tout ce qui n'est pas un `@include` ou un sélecteur imbriqué.

    ```scss
    .btn-green {
      background: green;
      font-weight: bold;
      // ...
    }
    ```

2. Déclarations `@include`

    Grouper les `@include` à la fin rend la lecture du sélecteur en entier plus facile.

    ```scss
    .btn-green {
      background: green;
      font-weight: bold;
      @include transition(background 0.5s ease);
      // ...
    }
    ```

3. Sélecteurs imbriqués

    Les sélecteurs imbriqués, _si nécessaire_, sont en dernier, et rien ne va après. Ajoutez un espace entre vos blocs de déclaration et sélecteurs imbriqués, ainsi qu'entre les sélecteurs imbriqués adjacents. Appliquez les mêmes directives que ci-dessus à vos sélecteurs imbriqués.

    ```scss
    .btn {
      background: green;
      font-weight: bold;
      @include transition(background 0.5s ease);

      .icon {
        margin-right: 10px;
      }
    }
    ```

### Variables

Préférez les noms de variables avec un tiret (par exemple `$my-variable`) plutôt que d'utiliser du camelCase ou snake_case. Il est acceptable de préfixer les noms de variables qui sont destinés à être utilisées uniquement dans le même fichier avec un trait de soulignement (par exemple `$_my-variable`).

### Mixins

Les mixins devrait être utilisé pour simplifier votre code (méthode « DRY »), ajouter de la clareté, ou abstraire de la complexité--de la même manière que les fonctions bien nommées. Les mixins qui n'acceptent pas d'argument peuvent être utile pour ça, mais notez que si vous ne compressez rien (par exemple Gzip), cela peut contribuer à la duplication de code inutile dans les feuilles de styles.

### Directive Extend

`@extend` doit être évité parce qu'il a un comportement non intuitif et potentiellement dangereux, surtout lorsqu'il est utilisé avec des sélecteurs imbriqués. Même étendre les sélecteurs de niveau supérieur peut causer des problèmes si l'ordre des sélecteurs change ultérieurement (par exemple, s'ils sont dans un autre fichier et que l'ordre d'appel de ce fichier à changé). Utiliser Gzip devrait gérer la plupart des économies que vous auriez gagnées avec `@extend`, et vous pouvez utiliser méthode « DRY » pour mieux alléger vos feuilles de styles à l'aide de mixins.

### Sélecteurs imbriqués

**Ne pas imbriquer les sélecteurs à plus de 3 niveaux de profondeur !**

```scss
.page-container {
  .content {
    .profile {
      // STOP!
    }
  }
}
```

Lorsque les sélecteurs deviennent longs, vous écrivez probablement du CSS qui est :

* Fortement couplé au HTML (faible) *—OU—*
* Trop spécifique (robuste) *—OU—*
* Non réutilisable


A nouveau : **ne jamais imbriquer des sélecteurs d'ID !**

Si vous devez utiliser un sélecteur d'ID en premier lieu (et vous ne devriez vraiment pas essayer), ils ne doivent pas être imbriqués. Si vous vous apercevez que vous le faites, vous devez revoir votre markup, ou trouver pourquoi une telle spécificité est nécessaire.
Si vous écrivez du HTML et du CSS bien formattés, vous ne devriez **jamais** avoir besoin de le faire.

**[⬆ Retour en haut](#table-des-matieres)**

## Traductions

  Ce guide de style est aussi disponible dans d'autres langues :

  - ![id](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Indonesia.png) **Bahasa Indonesia**: [mazipan/css-style-guide](https://github.com/mazipan/css-style-guide)
  - ![tw](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Taiwan.png) **Chinese (Traditional)**: [ArvinH/css-style-guide](https://github.com/ArvinH/css-style-guide)
  - ![cn](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/China.png) **Chinese (Simplified)**: [Zhangjd/css-style-guide](https://github.com/Zhangjd/css-style-guide)
  - ![fr](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/France.png) **French**: [mat-u/css-style-guide](https://github.com/mat-u/css-style-guide)
  - ![ja](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Japan.png) **Japanese**: [nao215/css-style-guide](https://github.com/nao215/css-style-guide)
  - ![ko](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/South-Korea.png) **Korean**: [CodeMakeBros/css-style-guide](https://github.com/CodeMakeBros/css-style-guide)
  - ![PT-BR](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Brazil.png) **Portuguese**: [felipevolpatto/css-style-guide](https://github.com/felipevolpatto/css-style-guide)
  - ![ru](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Russia.png) **Russian**: [Nekorsis/css-style-guide](https://github.com/Nekorsis/css-style-guide)
  - ![es](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Spain.png) **Spanish**: [ismamz/guia-de-estilo-css](https://github.com/ismamz/guia-de-estilo-css)
  - ![vn](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Vietnam.png) **Vietnamese**: [trungk18/css-style-guide](https://github.com/trungk18/css-style-guide)

**[⬆ Retour en haut](#table-des-matieres)**
