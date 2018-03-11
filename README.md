# Guia de estilo CSS / Sass da Airbnb

*Uma abordagem mais razoável para CSS e Sass*

## Índice

1. [Terminologia](#terminologia)
    - [Declaração de regra](#declaração-de-regra)
    - [Seletores](#seletores)
    - [Propriedades](#propriedades)
1. [CSS](#css)
    - [Formatação](#formatação)
    - [Comentários](#comentários)
    - [OOCSS e BEM](#oocss-e-bem)
    - [Seletores ID](#seletores-id)
    - [JavaScript hooks](#javascript-hooks)
    - [Border](#border)
1. [Sass](#sass)
    - [Sintaxe](#sintaxe)
    - [Ordenação](#ordenação-das-declarações-de-regra)
    - [Variáveis](#variáveis)
    - [Mixins](#mixins)
    - [Diretiva Extend](#diretiva-extend)
    - [Seletores aninhados](#seletores-aninhados)
1. [Tradução](#tradução)

## Terminologia

### Declaração de regra

A “declaração de regra” é o nome dado ao seletor (ou a um grupo de seletores) acompanhados de um grupo de propriedades. Segue um exemplo:

```css
.listing {
  font-size: 18px;
  line-height: 1.2;
}
```

### Seletores

Na declaraçao de um regra, os “seletores” são os bits que determinam quais elementos na árvore do DOM serão estilizados pelas propriedades definidas. Os seletores podem ser de elementos HTML, classes, ID ou qualquer um de seus atributos. Seguem alguns exemplos de seletores:

```css
.my-element-class {
  /* ... */
}

[aria-hidden] {
  /* ... */
}
```

### Propriedades

Finalmente, as propriedades definem o estilos dos seletores. As propriedades são pares chave-valor, onde uma declaração de regra pode conter uma ou mais declarações de propriedades.

```css
/* some selector */ {
  background: #f1f1f1;
  color: #333;
}
```

**[⬆ voltar ao início](#table-of-contents)**

## CSS

### Formatação

* Use *soft tabs* (2 espaços) para identação.
* Prefira dashes `(-)` em vez de camelCasing nos nomes das classes.
  - Underscores `(_)` e PascalCasing são aceites no caso de usar a metodologia BEM (veja [OOCSS e BEM](#oocss-and-bem) abaixo).
* Não use seletores ID.
* Quando usar múltiplos seletores numa declaração de regra, coloque um em cada linha.
* Coloque um espaço antes da chaveta de abertura `{` na declaração de regras.
* Nas propriedades, coloque um espaço depois, mas não antes do caractere `:`.
* Coloque a chaveta de fechamento `}` de uma declaração de regra numa nova linha.
* Coloque linhas em branco entre declarações de regra.

**Incorreto**

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

**Correto**

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

### Comentários

* Prefira comentários de linha (`//` em Sass) a comentários em bloco.
* Prefira comentários na sua própria linha. Evite comentários no final da linha.
* Escreva comentários detalhados para código que seja auto-documentados:
  - Usos do z-index
  - Compatibilidade ou *hacks* específicos de *browsers*

### OOCSS e BEM

Incentivamos algumas combinações de OOCSS e BEM por estas razões:

  * Ajuda a criar relações claras e estritas entre CSS e HTML
  * Ajuda-nos a criar componentes reutilizáveis e que podem ser combinados
  * Permite menor aninhamento e especificidade
  * Ajuda na construção de folhas de estilo escaláveis

**OOCSS**, ou “Object Oriented CSS”, é uma abordagem para escrita de CSS que o encoraja a pensar sobre as suas folhas de estilo como uma coleção de "objetos": *snippets* repetitivos reutilizáveis que podem ser usados de forma independente em todo o website.

  * [OOCSS wiki](https://github.com/stubbornella/oocss/wiki) de Nicole Sullivan
  
  * [Introduction to OOCSS](http://www.smashingmagazine.com/2011/12/12/an-introduction-to-object-oriented-css-oocss/) de Smashing Magazine

**BEM**, ou “Block-Element-Modifier”, é uma _convenção de nomes_ para classes em HTML e CSS. Foi originalmente desenvolvido pela Yandex com grandes bases de código e escalabilidade em mente, e pode servir como um sólido conjunto de diretrizes para a implementação do OOCSS.

  * [BEM 101](https://css-tricks.com/bem-101/) de CSS Trick
  * [Introduction to BEM](http://csswizardry.com/2013/01/mindbemding-getting-your-head-round-bem-syntax/) de Harry Roberts

Nós recomendamos um variante do BEM com “blocos” no formato *PascalCase*, que funciona particularmente bem quando o combinado com componentes (ex.: React). *Underscores* e *traços* continuam a ser utilizados para modificadores e filhos.


**Exemplo**

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

  * `.ListingCard` is the “block” and represents the higher-level component
  * `.ListingCard__title` is an “element” and represents a descendant of `.ListingCard` that helps compose the block as a whole.
  * `.ListingCard--featured` is a “modifier” and represents a different state or variation on the `.ListingCard` block.

### Seletores ID

While it is possible to select elements by ID in CSS, it should generally be considered an anti-pattern. ID selectors introduce an unnecessarily high level of [specificity](https://developer.mozilla.org/en-US/docs/Web/CSS/Specificity) to your rule declarations, and they are not reusable.

For more on this subject, read [CSS Wizardry's article](http://csswizardry.com/2014/07/hacks-for-dealing-with-specificity/) on dealing with specificity.

### JavaScript hooks

Avoid binding to the same class in both your CSS and JavaScript. Conflating the two often leads to, at a minimum, time wasted during refactoring when a developer must cross-reference each class they are changing, and at its worst, developers being afraid to make changes for fear of breaking functionality.

We recommend creating JavaScript-specific classes to bind to, prefixed with `.js-`:

```html
<button class="btn btn-primary js-request-to-book">Request to Book</button>
```

### Border

Use `0` instead of `none` to specify that a style has no border.

**Incorreto**

```css
.foo {
  border: none;
}
```

**Correto**

```css
.foo {
  border: 0;
}
```
**[⬆ voltar ao início](#table-of-contents)**

## Sass

### Sintaxe

* Use the `.scss` syntax, never the original `.sass` syntax
* Order your regular CSS and `@include` declarations logically (see below)

### Ordenação das declarações de regra

1. Property declarations

    List all standard property declarations, anything that isn't an `@include` or a nested selector.

    ```scss
    .btn-green {
      background: green;
      font-weight: bold;
      // ...
    }
    ```

2. `@include` declarations

    Grouping `@include`s at the end makes it easier to read the entire selector.

    ```scss
    .btn-green {
      background: green;
      font-weight: bold;
      @include transition(background 0.5s ease);
      // ...
    }
    ```

3. Nested selectors

    Nested selectors, _if necessary_, go last, and nothing goes after them. Add whitespace between your rule declarations and nested selectors, as well as between adjacent nested selectors. Apply the same guidelines as above to your nested selectors.

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

### Variáveis

Prefer dash-cased variable names (e.g. `$my-variable`) over camelCased or snake_cased variable names. It is acceptable to prefix variable names that are intended to be used only within the same file with an underscore (e.g. `$_my-variable`).

### Mixins

Mixins should be used to DRY up your code, add clarity, or abstract complexity--in much the same way as well-named functions. Mixins that accept no arguments can be useful for this, but note that if you are not compressing your payload (e.g. gzip), this may contribute to unnecessary code duplication in the resulting styles.

### Diretiva Extend

`@extend` should be avoided because it has unintuitive and potentially dangerous behavior, especially when used with nested selectors. Even extending top-level placeholder selectors can cause problems if the order of selectors ends up changing later (e.g. if they are in other files and the order the files are loaded shifts). Gzipping should handle most of the savings you would have gained by using `@extend`, and you can DRY up your stylesheets nicely with mixins.

### Seletores aninhados

**Do not nest selectors more than three levels deep!**

```scss
.page-container {
  .content {
    .profile {
      // STOP!
    }
  }
}
```

When selectors become this long, you're likely writing CSS that is:

* Strongly coupled to the HTML (fragile) *—OR—*
* Overly specific (powerful) *—OR—*
* Not reusable


Again: **never nest ID selectors!**

If you must use an ID selector in the first place (and you should really try not to), they should never be nested. If you find yourself doing this, you need to revisit your markup, or figure out why such strong specificity is needed. If you are writing well formed HTML and CSS, you should **never** need to do this.

**[⬆ voltar ao início](#table-of-contents)**

## Tradução

  This style guide is also available in other languages:

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
  - ![vn](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Italy.png) **Italian**: [antoniofull/linee-guida-css](https://github.com/antoniofull/linee-guida-css)

**[⬆ voltar ao início](#table-of-contents)**
