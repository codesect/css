# CodeSect CSS/Sass Style Guide

## Table of Contents

1. [Terminology](#terminology)
   - [Rule Declaration](#rule-declaration)
   - [Selectors](#selectors)
   - [Properties](#properties)
2. [CSS](#css)
   - [Formatting](#formatting)
   - [Comments](#comments)
   - [OOCSS and BEM](#oocss-and-bem)
   - [ID Selectors](#id-selectors)
   - [JavaScript hooks](#javascript-hooks)
   - [Border](#border)
3. [Sass](#sass)
   - [Syntax](#syntax)
   - [Ordering](#ordering-of-property-declarations)
   - [Variables](#variables)
   - [Mixins](#mixins)
   - [Extend directive](#extend-directive)
   - [Nested selectors](#nested-selectors)

## Terminology

### Rule declaration

A "rule declaration" is the name given to a selector (or a group of selectors) with an accompanying group of properties. Here's an example:

```css
.listing {
  font-size: 1.25rem;
  line-height: 1.5;
}
```

### Selectors

In a rule declaration, "selectors" are the bits that determine which elements in the DOM tree will be styled by the defined properties. Selectors can match HTML elements, as well as an element's class, ID, or any of its attributes. Here are some examples of selectors:

```css
.my-element-class {
  /* ... */
}

[aria-hidden] {
  /* ... */
}
```

### Properties

Finally, properties are what give the selected elements of a rule declaration their style. Properties are key-value pairs, and a rule declaration can contain one or more property declarations. Property declarations look like this:

```css
/* some selector */
 {
  background-color: #fafafa;
  color: #353535;
}
```

**[⬆ back to top](#table-of-contents)**

## CSS

### Formatting

- Use soft tabs (2 spaces) for indentation.
- Prefer dashes over camelCasing in class names.
  - Underscores and PascalCasing are okay if you are using BEM (see [OOCSS and BEM](#oocss-and-bem) below).
- Do not use ID selectors.
- When using multiple selectors in a rule declaration, give each selector its own line.
- Put a space before the opening brace `{` in rule declarations.
- In properties, put a space after, but not before, the `:` character.
- Put closing braces `}` of rule declarations on a new line.
- Put blank lines between rule declarations.
- End all declarations with a semicolon.
- Lowercase all hex values, e.g. `#fff`.
- Quote attribute values in selectors, e.g. `input[type="text"]`.
- Avoid specifying units for zero values, e.g. `margin: 0;` instead of `margin: 0px;`.
- Limit shorthand declaration usage to instances where you must explicitly set all available values.

**Bad**

```css
.avatar {
  background: crimson;
  border: 2px solid #FFF;
  border-radius: 50%;
  margin: 0px auto }
.no, .nope, .notGood {
  // ...
}
#lol-no {
  // ...
}
input[type=email] {
  // ...
}
```

**Good**

```css
.avatar {
  background-color: crimson;
  border: 2px solid #fff;
  border-radius: 50%;
  margin: 0 auto;
}

.one,
.selector,
.per-line {
  // ...
}

input[type="email"] {
  // ...
}
```

### Comments

- Prefer line comments (`//` in Sass-land) to block comments.
- Prefer comments on their own line. Avoid end-of-line comments.
- Write detailed comments for code that isn't self-documenting:
  - Uses of z-index
  - Compatibility or browser-specific hacks

### OOCSS and BEM

We encourage some combination of OOCSS and BEM for these reasons:

- It helps create clear, strict relationships between CSS and HTML
- It helps us create reusable, composable components
- It allows for less nesting and lower specificity
- It helps in building scalable stylesheets

**OOCSS**, or "Object-oriented CSS", is an approach for writing CSS that encourages you to think about your stylesheets as a collection of "objects": reusable, repeatable snippets that can be used independently throughout a website.

- Nicole Sullivan's [OOCSS wiki](https://github.com/stubbornella/oocss/wiki)
- Smashing Magazine's [Introduction to OOCSS](http://www.smashingmagazine.com/2011/12/12/an-introduction-to-object-oriented-css-oocss/)

**BEM**, or "Block-Element-Modifier", is a _naming convention_ for classes in HTML and CSS. It was originally developed by Yandex with large codebases and scalability in mind, and can serve as a solid set of guidelines for implementing OOCSS.

- CSSTricks' [BEM 101](https://css-tricks.com/bem-101/)
- Harry Roberts' [introduction to BEM](http://csswizardry.com/2013/01/mindbemding-getting-your-head-round-bem-syntax/)

We recommend a variant of BEM with PascalCased "blocks", which works particularly well when combined with components (e.g. React). Underscores and dashes are still used for modifiers and children.

**Example**

```jsx
// ListingCard.js
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
.ListingCard {
}

.ListingCard--featured {
}

.ListingCard__title {
}

.ListingCard__content {
}
```

- `.ListingCard` is the "block" and represents the higher-level component.
- `.ListingCard__title` is an "element" and represents a descendant of `.ListingCard` that helps compose the block as a whole.
- `.ListingCard--featured` is a "modifier" and represents a different state or variation on the `.ListingCard` block.

### ID selectors

While it is possible to select elements by ID in CSS, it should generally be considered an _anti-pattern_. ID selectors introduce an unnecessarily high level of [specificity](https://developer.mozilla.org/en-US/docs/Web/CSS/Specificity) to your rule declarations, and they are not reusable.

For more on this subject, read [CSS Wizardry's article](http://csswizardry.com/2014/07/hacks-for-dealing-with-specificity/) on dealing with specificity.

### JavaScript hooks

Avoid binding to the same class in both your CSS and JavaScript. Conflating the two often leads to, at a minimum, time wasted during refactoring when a developer must cross-reference each class they are changing, and at its worst, developers being afraid to make changes for fear of breaking functionality.

We recommend creating JavaScript-specific classes to bind to, prefixed with `.js-`:

```html
<button class="btn btn--primary js-request-to-book">Request to Book</button>
```

### Border

Use `0` instead of `none` to specify that a style has no border.

**Bad**

```css
.foo {
  border: none;
}
```

**Good**

```css
.foo {
  border: 0;
}
```

**[⬆ back to top](#table-of-contents)**

## Sass

### Syntax

- Use the `.scss` syntax, not the original `.sass` syntax.
- Order your regular CSS and `@include` declarations logically (see below).
- Wrap all math operations in parentheses with a single space between values, variables, and operators.

### Ordering of property declarations

1. Property declarations

   List all standard property declarations, anything that isn't an `@include` or a nested selector.

   ```scss
   .btn-green {
     color: white;
     font-weight: 600;
     // ...
   }
   ```

2. `@include` declarations

   Grouping `@include`s at the end makes it easier to read the entire selector.

   ```scss
   .btn-green {
     color: white;
     font-weight: 600;
     @include gradient-bg(seagreen, yellowgreen);
     // ...
   }
   ```

3. Nested selectors

   Nested selectors, _if necessary_, go last, and nothing goes after them. Add whitespace between your rule declarations and nested selectors, as well as between adjacent nested selectors. Apply the same guidelines as above to your nested selectors.

   ```scss
   .btn {
     color: white;
     font-weight: 600;
     @include gradient-bg(seagreen, yellowgreen);

     .icon {
       margin-right: 0.625rem;
     }
   }
   ```

### Variables

Prefer dash-cased variable names (e.g. `$my-variable`) over camelCased or snake_cased variable names. It is acceptable to prefix variable names that are intended to be used only within the same file with an underscore (e.g. `$_my-variable`).

### Mixins

Mixins should be used to DRY up your code, add clarity, or abstract complexity—in much the same way as well-named functions. Mixins that accept no arguments can be useful for this, but note that if you are not compressing your payload (e.g. gzip), this may contribute to unnecessary code duplication in the resulting styles.

### Extend directive

`@extend` should be avoided because it has unintuitive and potentially dangerous behaviour, especially when used with nested selectors. Even extending top-level placeholder selectors can cause problems if the order of selectors ends up changing later (e.g. if they are in other files and the order the files are loaded shifts). Gzipping should handle most of the savings you would have gained by using `@extend`, and you can DRY up your stylesheets nicely with mixins.

### Nested selectors

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

- Strongly coupled to the HTML (fragile) _—OR—_
- Overly specific (powerful) _—OR—_
- Not reusable

Again: **never nest ID selectors!**

If you must use an ID selector in the first place (and you should really try not to), they should never be nested. If you find yourself doing this, you need to revisit your markup or figure out why such strong specificity is needed. If you are writing well-formed HTML and CSS, you should **never** need to do this.

**[⬆ back to top](#table-of-contents)**
