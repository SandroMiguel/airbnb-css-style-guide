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

**[⬆ voltar ao início](#indice)**

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

Embora seja possível selecionar elementos por ID no CSS, geralmente deve ser considerado um *anti-pattern*. Seletores ID introduzem um elevado nível de [especificidade](https://developer.mozilla.org/en-US/docs/Web/CSS/Specificity) para aa declarações de regra e não são reutilizáveis.

For more on this subject, read [CSS Wizardry's article](http://csswizardry.com/2014/07/hacks-for-dealing-with-specificity/) on dealing with specificity.
Para mais informações sobre este assunto, leia o [artigo do CSS Wizardry](http://csswizardry.com/2014/07/hacks-for-dealing-with-specificity/) no tratamento da especificidade.

### JavaScript hooks

Evite vincular a mesma classe no CSS e no JavaScript. No mínimo, tal leva ao desperdício de tempo durante o *refactoring*, por exemplo, quando um programador deve fazer referência cruzada a cada classe que está a alterar e, no pior dos casos, leva os programadores a resistirem a fazer mudanças com receio de romper a funcionalidade.

Recomendamos a criação de classes específicas para JavaScript, com o prefixo `.js-`:

```html
<button class="btn btn-primary js-request-to-book">Request to Book</button>
```

### Border

Use o `0` em vez de `none` para especificar que um estilo não tem borda.

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
**[⬆ voltar ao início](#índice)**

## Sass

### Sintaxe

* Use a sintaxe `.scss`, nunca a sintaxe original `.sass`
* Order your regular CSS and `@include` declarations logically (see below)
* Ordene o CSS e o `@include` com lógica (veja abaixo)

### Ordenação das declarações de regra

1. Declaração de propriedades

    Listar todas as declarações de propriedades padrão, tudo o que não seja um `@include` ou um seletor aninhado.

    ```scss
    .btn-green {
      background: green;
      font-weight: bold;
      // ...
    }
    ```

2. Declarações `@include`

    Agrupar os `@include`s no final torna mais fácil a leitura de todo o seletor.

    ```scss
    .btn-green {
      background: green;
      font-weight: bold;
      @include transition(background 0.5s ease);
      // ...
    }
    ```

3. Seletores aninhados

    Nested selectors, _if necessary_, go last, and nothing goes after them. Add whitespace between your rule declarations and nested selectors, as well as between adjacent nested selectors. Apply the same guidelines as above to your nested selectors.
    Os seletores aninhados, _se necessário_, são colocados no final e nada mais depois deles. Adicione um espaço entre a declaração de regra e os seletores aninhados, assim como entre os seletores aninhados adjacentes. Aplique as mesmas diretrizes acima para os seletores aninhados.

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

Tenha preferência por nomes de variáveis em *dash-case* (ex.: `$my-variable`) em vez de *camelCase* ou *snake_case*. É aceitável adicionar um "underscore" como prefíxo nos nomes de variáveis que se destinam a ser usadas dentro do mesmo arquivo  (ex.: `$_my-variable`).

### Mixins

Os Mixins devem ser usados para implementar no código o princípio DRY, adicionar clareza ou abstrair-nos da complexidade, assim como as funções bem nomeadas. Os Mixins que não aceitam argumentos podem ser úteis, mas tenha atenção que se não compactar o seu *payload* (ex.: gzip) isto pode contribuir para duplicação de código desnecessário nos estilos resultantes.

### Diretiva Extend

O `@extend` deve ser evitado porque possui um comportamento não intuitivo e perigoso, especialmente quando usado nos seletores aninhados. Mesmo extendendo os seletores *placeholder* de nível superior, isto pode causar problemas se a ordem dos seletores mudar mais tarde (ex.: se estiverem noutros arquivos e a ordem de carregamento alternar). Ao usar o Gzip terá a maioria das economias que teria ganho usando `@extend`, e, além disso, pode tirar partido do princípio DRY através dos mixins.

### Seletores aninhados

**Não use seletores aninhados com mais de três níveis de profundidade!**

```scss
.page-container {
  .content {
    .profile {
      // STOP!
    }
  }
}
```

Quando os seletores se tornam assim tão longos, provavelmente estará a escrever CSS assim:

* Fortemente aclopado ao HTML (frágil) *—OU—*
* Excessivamente específico (poderoso) *—OU—*
* Não reutilizável


Mais uma vez: **nunca agrupe seletores ID!**

Se precisar de utilizar um seletor ID (coisa que você não deve fazer!), ele nunca deve ser agrupado. Se está fazer isso, é necessário rever o seu código ou descobrir por que é necessária tal especificidade. Se estiver a escrever HTML e CSS corretamente, **nunca** deverá ser necessário fazer isso!

**[⬆ voltar ao início](#índice)**

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

**[⬆ voltar ao início](#índice)**
