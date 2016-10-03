# CSS Style Guide

_Coding standards for stylesheets_

Audience: non-CSS developers writing HTML.

Related: **[Live Style Guide](https://m3a.vhlcentral.com/music/style_guide/)** (Components) |  **[Wiki](https://github.com/vhl/music/wiki)** (Concepts)

## Introduction

The purpose of this document is to provide guidelines for writing CSS. Code conventions are important for the long-term maintainability of code. Most of the time, developers are maintaining code, either their own or someone else’s. The goal is to have everyone’s code look the same, which allows any developer to easily work on another developer’s code.


<!-- MarkdownTOC -->

  - [Quick Summary of OOCSS:](#quick-summary-of-oocss)
- [Class Naming Conventions](#class-naming-conventions)
  - [All classnames should have a prefix](#all-classnames-should-have-a-prefix)
  - [Use BEM to reflect component structure](#use-bem-to-reflect-component-structure)
    - [Use Hyphens between words in a block classname](#use-hyphens-between-words-in-a-block-classname)
    - [Blocks](#blocks)
    - [Elements](#elements)
    - [Modifiers](#modifiers)
- [Coding Style](#coding-style)
  - [Indentation](#indentation)
  - [Brace Alignment](#brace-alignment)
  - [Selectors](#selectors)
    - [One selector per line](#one-selector-per-line)
    - [Keep specificity low.](#keep-specificity-low)
    - [Keep depth of applicability low.](#keep-depth-of-applicability-low)
  - [Properties](#properties)
  - [No Vendor-Prefixed Properties](#no-vendor-prefixed-properties)
  - [Using !important](#using-important)
  - [Using CSS Preprocessors](#using-css-preprocessors)
    - [Keep nesting to 3 levels deep, 4 absolute max.](#keep-nesting-to-3-levels-deep-4-absolute-max)
  - [Separate multiple classes with TWO spaces for readability](#separate-multiple-classes-with-two-spaces-for-readability)
  - [Units](#units)
    - [Do not use units with zero values](#do-not-use-units-with-zero-values)
  - [HEX values](#hex-values)
  - [String Literals](#string-literals)
  - [URLs](#urls)
  - [Attribute values in selectors](#attribute-values-in-selectors)
  - [JavaScript and Test Dependence](#javascript-and-test-dependence)
  - [:hover and :focus](#hover-and-focus)
  - [Comments](#comments)
- [Object-Oriented CSS \(OOCSS\)](#object-oriented-css-oocss)
  - [Composition with Multiple Classes](#composition-with-multiple-classes)
  - [Quick Summary of OOCSS:](#quick-summary-of-oocss-1)
- [Class Naming Conventions](#class-naming-conventions-1)
  - [All classnames should have a prefix](#all-classnames-should-have-a-prefix-1)
    - [c- Component](#c--component)
    - [is-, has- States](#is--has--states)
    - [t- Theme](#t--theme)
    - [u- utility](#u--utility)
    - [ns- Namespace.](#ns--namespace)
    - [js- Javascript.](#js--javascript)
    - [test- Test.](#test--test)
  - [Use BEM to reflect component structure](#use-bem-to-reflect-component-structure-1)
    - [Use Hyphens between words in a block classname](#use-hyphens-between-words-in-a-block-classname-1)
    - [Blocks](#blocks-1)
    - [Elements](#elements-1)
    - [Modifiers](#modifiers-1)
- [Feature-specific styles](#feature-specific-styles)
- [Style Documentation](#style-documentation)

<!-- /MarkdownTOC -->


<a name="quick-summary-of-oocss"></a>
### Quick Summary of OOCSS:

* **Single Responsibility Principle**: A class should do one specific thing.
* **Separate Structure from Skin**: Basically, use separate classes for Layout vs Style.
* **Avoid location-based styles**: Descendant selectors should be used sparingly; they should never reach outside the module in question.
* **Separate Containers from Content**: A component should have no knowledge of its container.
* **Component Gets its Width from its Container**: Avoid setting explicit widths on components; Grids control width.
* **Component Gets its Height from its Contents**: Avoid setting explicit heights on components; Content determines height.
* **Keep specificity Low**: Most selectors should consist of a single class.
  * **Don't use IDs as CSS selectors**: They are troublesome to override.
  * **Don't use elements in your selectors**: They limit reuse.
  * **Avoid too much overriding of styles**: Check the cascade in browser dev tools.

<a name="class-naming-conventions"></a>
## Class Naming Conventions




<a name="all-classnames-should-have-a-prefix"></a>
### All classnames should have a prefix

Prefixes help insulate BEM classes from any legacy classnames. The different prefixes are usually a single letter, and are explained in the [music gem documentation](). The system used here is adapted from [SMACSS](https://smacss.com/).

```css
/* Good */
.c-breadcrumbs {}

/* Bad - no prefix */
.breadcrumbs {}
```



<a name="use-bem-to-reflect-component-structure"></a>
### Use BEM to reflect component structure

BEM convention (Block, Element, Modifier) uses different delimiters to make it easier to understand the role of an element.



<a name="use-hyphens-between-words-in-a-block-classname"></a>
#### Use Hyphens between words in a block classname

```css
/* Good */
.c-data-table {}

/* Bad - uses single underscore. */
.c_data_table {}

/* Bad - uses camel-case. */
.c-listInlineBoxy {}
```



<a name="blocks"></a>
#### Blocks

A **Block** (The parent element in an object or component) uses hyphens between words. Prefix all class names with the first letter of their type: o- Object, c- Component, l-Layout, u- Utility. See the [README](https://github.com/vhl/music/blob/library/app/assets/stylesheets/music/README.md) in the music gem library for more details.

```css
/* Good */
.o-list-inline {}
.o-list-inline__list-item {}
.c-list-inline--boxy {}

/* Bad - uses camel case*/
.u-pullLeft {}

/* Bad - uses single underscores */
.c-list_inline {}
```



<a name="elements"></a>
#### Elements

Elements are child elements of a component. They add a suffix to the base name, separated by double underscores.

```css
/* Good: double underscore */
.c-list-inline__item {}

/* Bad - looks like a block. */
.c-list-inline-item {}
```



<a name="modifiers"></a>
#### Modifiers

Modifiers are subclasses, or variants, of a component. They add a suffix to the base name, separated by double hyphens.

```css
/* Good: double hyphen */
.c-list-inline--boxy {}

/* Bad - looks like a block. */
.c-list-inline-boxy {}
```

When extending a component and styling the inner elements, use the base component's inner elements' class name for styling, instead of extending the class names of the inner elements as well.

```css

/* Good - modifiers (subclasses) of components refer to unmodified inner element's names */
.c-list-inline--boxy > .o-list-inline__list-item {}

/* Bad - don't modify inner element's names */
.c-list-inline--boxy > .c-list-inline__list-item--boxy {}

/* Bad - don't create child elements with the modifier in the name. */
.c-list-inline--boxy__list-item {}
```



>>>>>>> Stashed changes
<a name="coding-style"></a>
## Coding Style

<a name="indentation"></a>
### Indentation

Each indentation level is made up of two spaces. Do not use tabs. (Please set your editor to use two spaces.)

```css
/* Good */
.c-message {
  color: #fff;
  background-color: #000;
}

/* Bad - all on one line */
.c-message {color: #fff; background-color: #000;}
```

In the case of single selector, single property rules, one line is acceptable.
Leave one space inside the brackets.

```
/* Fine - one selector, one property */
.c-alert { font-weight: bold; }
```


Rules inside of `@media` must be indented an additional level.

```css
/* Good */
@media screen and (max-width:480px) {
  .c-message {
    color: green;
  }
}
```

Strict BEM says to indent child elements, but this can easily be confused for nesting. Keep child elements at the same indentation level as their parent (unless they actually _are_ nested for specificity reasons). **NOTE: Considering reinstating indentation of child elements.**


<a name="brace-alignment"></a>
### Brace Alignment

The opening brace should be on the same line as the last selector in the rule and should be preceded by a space. The closing brace should be on its own line after the last property and be indented to the same level as the line with the opening brace.

```css
/* Good */
.c-description {
  color: #fff;
}

/* Bad - closing brace is in the wrong place */
.c-description {
  color: #fff;
  }

/* Bad - opening brace missing space */
.c-description{
  color: #fff;
}
```


<a name="selectors"></a>
### Selectors

<a name="one-selector-per-line"></a>
#### One selector per line

Each selector should appear on its own line. The line should break immediately after the comma. Each selector should be aligned to the same left column.

```css
/* Good */
button,
input.c-button {
  color: red;
}

/* Bad - selectors on one line */
button, input.c-button {
  color: red;
}
```


<a name="keep-specificity-low"></a>
#### Keep specificity low.

* Type selectors (addressing elements by their tag name) should be defined early in the cascade and then never used again unless absolutely necessary.

* **Selectors consisting of a single class** should make up the majority of your CSS. A second class should generally only be added for states. 

* **NEVER use id's in selectors**. They have very high specificity compared to classes, and can't be used more than once on the same page.

* For the same reason, **NEVER use inline styles**. They are near-impossible to override without resorting to JavaScript.

* It should go without saying, **don't use `!important`** except in rare cases:
  * Utilities use it to override any previous values.
  * Overriding any high-specificity styles we don't have control over.


<a name="keep-depth-of-applicability-low"></a>
#### Keep depth of applicability low.

Keep combinators (whitespace, `>`, `+`, `~`,) to a minimum. Favor direct-child combinator (`>`) over descendant (whitespace).

`.c-super-table a {...}` can get you in trouble later when you want to override that link style using `.special` -- better to add a `.c-super-table__link` in the first example to keep both depth of applicability and specificity low. A single class selector can always be overridden by a single class selector later in the cascade.


<a name="properties"></a>
### Properties

Each property must be on its own line and indented one level from the selector. There should be no space before the colon and one space after. All properties must end with a semicolon.

```css
/* Good */
.c-story {
    background-color: blue;
    color: red;
}

/* Bad - missing spaces after colons */
.c-story {
    background-color:blue;
    color:red;
}

/* Bad - missing last semicolon */
.c-story {
    background-color: blue;
    color:red
}
```


<a name="no-vendor-prefixed-properties"></a>
### No Vendor-Prefixed Properties

Don't use vendor prefixes like `-webkit-border-radius`, just use the unprefixed form. Autoprefix will be run on all the styles when they are compiled, and necessary prefixed properties will be added to the final output CSS automatically.

```css
/* Good */
.c-directory {
  border-radius: 4px;
}

/* Bad */
.c-directory {
  -webkit-border-radius: 4px;
  -moz-border-radius: 4px;
  border-radius: 4px;
}
```

Suffix property value pairs that apply only to a particular browser or class of browsers with a comment listing browsers affected.

```css
background: #fcfcfc; /* Old browsers */
background: url('fills/texture-1.jpg') /* IE 8- */
background: url('fills/texture-1.jpg'), url('fills/texture-2.png'); /* W3C */
```

Suffix fallback with “Old browsers” and standard property with “W3C”. Add a plus or minus to indicate that a property applies to all previous browsers by the same vendor or all future browsers by the same vendor.


<a name="using-important"></a>
### Using !important

Do not use !important on CSS properties. The only time this is allowed is in a u- utility style (provided by Core team).

```css
/* Good */
.c-story__heading {
   color: red;
}

/* Bad - don't use !important */
.c-story__heading {
   color: red !important;
}
```



<a name="using-css-preprocessors"></a>
### Using CSS Preprocessors

<a name="keep-nesting-to-3-levels-deep-4-absolute-max"></a>
#### Keep nesting to 3 levels deep, 4 absolute max.

```css
/* Good */
.stubbornella {
    .inner {
      ...

        .title {
         ....

        }
    }
}

/* Bad - more than 3 levels of nesting */
.stubbornella {
    .inner {
      ...

        .title {
         ....

            .subtxt {
                ...

            }
        }
    }
}
```


<a name="separate-multiple-classes-with-two-spaces-for-readability"></a>
### Separate multiple classes with TWO spaces for readability

```html
<!-- Good -->
<div class="c-inline-list  c-tutorial-nav  u-pull-left">

<!-- Bad - don't use single space -->
<div class="c-inline-list c-tutorial-nav u-pull-left">

<!-- Bad - not ordered by object inheritance -->
<div class="c-tutorial-nav  u-pull-left  o-inline-list">
```


<a name="units"></a>
### Units

In general, use mod() -- this is a custom SASS function that provides measures relative to a common standard. The value of mod(1) is 1rem. Rems are like ems, but without cascading problems: if a rem = 16px, you can count on that value anywhere in the page.

In order to keep things in sensible proportions, as well as improve readability, write mod values in whole numbers or simple fractions.

Put one space on either side of an operator in an expression.

```css
/* Good */
margin: mod(1);
padding: mod(1 / 2);

/* Bad: uses rems directly */
margin: 0.75rem;
padding: 1rem;

/* Bad: uses fractions unrelated to the base unit,
   and no spaces around operator: */
padding: mod(3/5);
```

mod() should be used for all margins, padding, gutters, font sizes, etc.

Pixel lengths are discouraged, but not blanket verboten! For little tweaky things like box-shadow offsets or `margin: -1px`, they are fine.

Ems should be used very rarely -- example: if you need to add a bit of simulated small-caps inside a heading. We don't care which heading level it is, so we specify it relative to the parent with ems. You wouldn't want to use them on an element that has further levels of hierarchy.


<a name="do-not-use-units-with-zero-values"></a>
#### Do not use units with zero values

Zero values do not require named units, omit the “px” or other unit.

```css
/* Good */
.c-story {
   margin: 0;
}

/* Bad - uses units */
.c-story {
   margin: 0px;
}
```


<a name="hex-values"></a>
### HEX values

When specifying color values in HEX, use lowercase, and if possible, 3-character shorthand:

```css
/* Good */
.c-story__credit {
  color: #ccc;
  background-color: #1b48fa;
}

/* Bad */
.c-story__credit {
  color: #CCCCCC;
  background-color: #1B48FA;
}
```


<a name="string-literals"></a>
### String Literals

Strings should always use single quotes (never double quotes).

```css
/* Good: single quotes */
.c-story__credit:after {
  content: 'Stubbornella';
  background: transparent url('beachball.png');
}

/* Bad - double quotes */
.c-story__credit:after {
  content: "Stubbornella";
  background: transparent url("beachball.png");
}
```


<a name="urls"></a>
### URLs

When using a url() value, always use single quotes around the actual URL.

```css
/* Good */
.c-header {
  background: url('img/logo.png');
}

/* Bad - double quotes */
.c-header {
  background: url("img/logo.png");
}

/* Bad - missing quotes */
.c-header {
  background: url(img/logo.png);
}
```


<a name="attribute-values-in-selectors"></a>
### Attribute values in selectors

Use double quotes around attribute values.

```css
/* Good */
input[type="submit"] {}

/* Bad - using single quote */
input[type='submit'] {}

/* Bad - missing quotes */
input[type=submit] {}
```


<a name="javascript-and-test-dependence"></a>
### JavaScript and Test Dependence

If an item is manipulated by Javascript, it should be have a js- class purposes of selecting that element. Javascript should only query elements by js- classes. CSS should not use js- classnames in its selectors.

```html
<!-- Good -->
<div class="c-tabset  js-tabset"></div>
<script> tabset = $('.js-tabset'); </script>

<!-- Bad: -->
<div class="c-tabset"></div>
<script> tabset = $('.c-tabset'); </script>
```

Likewise, automated tests need a test- class prefix and should select only by that classname, never by styling classes or js- classes.


<a name="hover-and-focus"></a>
### :hover and :focus

If :hover pseudo class is styled, :focus should also be styled for accessibility. Focus styles should never be removed entirely from any focusable element.

```css
/* Good */
a:hover,
a:focus {
   color: green;
}

/* Bad - missing :focus */
a:hover {
   color: green;
}
```


<a name="comments"></a>
### Comments

```css
/* ==========================================================================
    Section comment block
   ========================================================================== */

/*

    Sub-section comment block

*/

/* Basic one-line comment */
```


<a name="object-oriented-css-oocss"></a>
## Object-Oriented CSS (OOCSS)

OOCSS advocates practices which are beneficial to flexibility, code reuse, maintenance, and performance. For more info, see the [OOCSS wiki](https://github.com/stubbornella/oocss/wiki).


<a name="composition-with-multiple-classes"></a>
### Composition with Multiple Classes

One of the core ideas of Object-Oriented CSS is using multiple classnames on a single HTML element to synthesize new components from small, modular, focused styles. 

Separate multiple classes with TWO spaces for readability:

```html
<!-- Good -->
<div class="c-tutorial-nav  c-tutorial-nav--alt  u-pull-left">

<!-- Bad - don't use single space -->
<div class="c-tutorial-nav c-tutorial-nav--alt u-pull-left">

<!-- Bad - not ordered by inheritance: "alt" extends the base. -->
<div class="c-tutorial-nav--alt  c-tutorial-nav  u-pull-left">
```

The classes should follow this order:

```html
<div class="[block|element]  [modifier  [modifier]]  [state]  [utility]  [javascript hooks]  [test hooks]">
```


<a name="quick-summary-of-oocss-1"></a>
### Quick Summary of OOCSS:

* **Single Responsibility Principle**: A class should do one specific thing.
* **Separate Structure from Skin**: Basically, use separate classes for Layout vs Style.
* **Avoid location-based styles**: Descendant selectors should be used sparingly; they should never reach outside the module in question.
* **Separate Containers from Content**: A component should have no knowledge of its container.
* **Component Gets its Width from its Container**: Avoid setting explicit widths on components; Grids control width.
* **Component Gets its Height from its Contents**: Avoid setting explicit heights on components; Content determines height.
* **Keep specificity Low**: Most selectors should consist of a single class.
  * **Don't use IDs as CSS selectors**: They are troublesome to override.
  * **Don't use elements in your selectors**: They limit reuse.
  * **Avoid too much overriding of styles**: Check the cascade in browser dev tools.


<a name="class-naming-conventions-1"></a>
## Class Naming Conventions

<a name="all-classnames-should-have-a-prefix-1"></a>
### All classnames should have a prefix

Prefixes, along with BEM classnames, help insulate classes from any legacy classnames. The different prefixes are usually a single letter, and are explained in the [music gem documentation](). The system used here is adapted from [SMACSS](https://smacss.com/).

```css
/* Good */
.c-breadcrumbs

/* Bad - no prefix */
.breadcrumbs
```


<a name="c--component"></a>
#### c- Component
This is a UI-specific implementation of a design pattern. Editing these styles is
relatively safe, in that only instances of this component will be affected.
Examples: Table of Contents, Work Set, Flashcard.


<a name="is--has--states"></a>
#### is-, has- States

Specifies that the components currently is in a
particular (temporary) state. Examples: .is-expanded, .has-lessons,
.is-published


<a name="t--theme"></a>
#### t- Theme

Set at a high level (body or top of view). Used to switch color palettes and possibly some other SASS variables. We will use a theme as part of SuperSite vs Portales differentiation.


<a name="u--utility"></a>
#### u- utility

Does something simple that is just a quick override,
e.g., .txtR to align text right for a particular element. Usually they
are defined with a single style rule. You shouldn't be using these a lot.
These styles should not be changed, since they will have side-effects in
unpredictable places.


<a name="ns--namespace"></a>
#### ns- Namespace.

`ns-newcsslib` is used to insulate pages or sections so that styles from this library apply there, but not anywhere else. Any other namespace classes should be used extremely sparingly. They are meant to be temporary.


<a name="js--javascript"></a>
#### js- Javascript.

Used for scripting hooks only. Scripts should not make reference to any other classes.


<a name="test--test"></a>
#### test- Test.

Used for automated testing hooks only. Test scripts should not make reference to any other classes.


<a name="use-bem-to-reflect-component-structure-1"></a>
### Use BEM to reflect component structure

BEM convention (Block, Element, Modifier) uses different delimiters to make it easier to understand the role of an element.
 

<a name="use-hyphens-between-words-in-a-block-classname-1"></a>
#### Use Hyphens between words in a block classname
 
```css
/* Good */
.c-data-table {}
 
/* Bad - uses single underscore. */
.c_data_table {}
 
/* Bad - uses camel-case. */
.c-listInlineBoxy {}
```
 

<a name="blocks-1"></a>
#### Blocks

A **Block** (The parent element of a component) uses hyphens between words. [Prefix all class names](#all-classnames-should-have-a-prefix).

We usually refer to these as **base classes**.

```css
/* Good */
.c-list-inline {}
.c-assignment-list__item {}
.c-list--boxy {}

/* Bad - uses camel case */
.u-pullLeft {}

/* Bad - uses single underscores */
.c-list_inline {}
```


<a name="elements-1"></a>
#### Elements

Elements are child elements of a component. They add a suffix to the base name, separated by double underscores.

```css
/* Good: double underscore */
.c-list-inline__item {}

/* Bad - looks like a block. */
.c-list-inline-item {}
```
 

<a name="modifiers-1"></a>
#### Modifiers
 
Modifiers are "subclasses", or variants, of a component. They add a suffix to the base name, separated by double hyphens.
 
```css
/* Good: double hyphen */
.c-list-inline--boxy {}

/* Bad - looks like a block. */
.c-list-inline-boxy {}
```
 
When extending a component and styling the inner elements, use the base component's inner elements' class name for styling, instead of extending the class names of the inner elements as well.

Use a descendant selector (preferably direct child `>` ) with the modified base classname.

```css

/* Good - modifiers (subclasses) of components refer to unmodified inner element's names */
.c-assignment-list--boxy > .c-assignment-list__item {}

/* Bad - don't modify inner element's names */
.c-assignment-list--boxy > .c-assignment-list__list-item--boxy {}

/* Bad - don't create child elements with the modifier in the name. */
.c-assignment-list--boxy__list-item {}
```
 
 
<a name="feature-specific-styles"></a>
## Feature-specific styles
 
If it is uncertain whether a new class belongs in the library (i.e., will be re-used, and NEVER changed), then it should be a feature class.
 
* Feature styles go in stylesheets/music/features. For any feature that doesn't yet exist as a subfolder, add one for your styles. 
 
* Feature style classnames adhere to the same guidelines as any others, just make sure to NEVER give a feature class the same name as a library class.

  * The practice of using `f-` to prefix feature styles has been deprecated. Use `c-` or whatever prefix is applicable.
 
* Adding a modifier to a class that exists in the library is encouraged: e.g., if `c-module` exists in the library, you can add a style in your "Gradebook v2" feature that defines `c-module--gradebook`, provided that class does not already exist in the library. 
 
* For music v1 only: All feature stylesheets are included in the main stylesheet, so there will no longer be a need to import them on a per view basis.


<a name="style-documentation"></a>
## Style Documentation

We use multiple strategies to make sure the CSS library is understandable and maintainable:

* SMACSS prefixes indicate different types of objects.
* BEM classnames make structure clear on both the HTML and CSS sides.
* Components and other classes are documented in specially formatted comments directly in the CSS/SASS files (see sample below).
* We use [Hologram](trulia.github.io/hologram/) to auto-generate a [live style guide](//M3-SERVER/music/style_guide) from these comments, which provides a browsable inventory of components, their variants, instructions on how to use them and what they should look like.

```css
<a name="doc"></a>
/*doc
---
title: Layout Grids
name: grid
<a name="category-layout"></a>
category: Layout
---

_The body text is ideal for more detailed explanations and
documentation. It should include a functional example of HTML for the classes in question._

_Be sure to indicate what classes and/or elements are extended by this class:_

Extends `ur-grid`

Extends `<a href>`


TODO: This is a todo statement that describes an atomic task to be completed
  at a later date. It wraps after 80 characters and following lines are
  indented by 2 spaces.
*/
```

In the doc header (the "front matter" between triple hyphens):

* *title* and *category* values should be capitalized.
* *name* values should be lowercase, with hyphens as word separators. Component docs are sorted by name, so plan accordingly.
* Don't include class prefixes in titles, names, or categories.
* Try to keep category names to a single word.

You can organize docs hierarchically by specifying a *parent* instead of a category. Such "child" elements will be listed in alphabetic order below the parent and inherit the parent's category. The parent value should match the *name* of another component.

```css
<a name="doc-1"></a>
/*doc
---
title: Inner Grid
name: grid-inner
<a name="parent-grid"></a>
parent: grid
---
*/
```
