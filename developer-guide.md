# CSS Developer Guide

_Coding standards for stylesheets_

Audience: CSS developers writing new styles & components.

Related: **[Live Style Guide](https://m3a.vhlcentral.com/music/style_guide/)** (Components) |  **[Wiki](https://github.com/vhl/music/wiki)** (Concepts)

## Introduction

The purpose of this document is to provide advanced guidelines for writing CSS. Code conventions are important for the long-term maintainability of code. Most of the time, developers are maintaining code, either their own or someone else’s. The goal is to have everyone’s code look the same, which allows any developer to easily work on another developer’s code.

We use a component-oriented CSS library called Music, based on OOCSS principles and BEM and SMACSS naming conventions. While OOCSS and BEM _happen_ to have code libraries associated with them, the value is primarily in the concepts and methodology.

For more info, see:

* [OOCSS Wiki](https://github.com/stubbornella/oocss/wiki).
* [BEM](https://en.bem.info/methodology/).
* [SMACSS](https://smacss.com/).


<!-- MarkdownTOC -->

- [Quick Summary of OOCSS:](#quick-summary-of-oocss)
- [Class Naming Conventions](#class-naming-conventions)
  - [All classnames should have a prefix](#all-classnames-should-have-a-prefix)
  - [Use BEM to reflect component structure](#use-bem-to-reflect-component-structure)
    - [Use Hyphens between words in a block classname](#use-hyphens-between-words-in-a-block-classname)
    - [Blocks](#blocks)
    - [Elements](#elements)
    - [Modifiers](#modifiers)
- [Using CSS Preprocessors](#using-css-preprocessors)
  - [Keep nesting to 3 levels deep, 4 absolute max.](#keep-nesting-to-3-levels-deep-4-absolute-max)
- [Style Documentation](#style-documentation)

<!-- /MarkdownTOC -->


<a name="quick-summary-of-oocss"></a>
## Quick Summary of OOCSS:

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


<a name="using-css-preprocessors"></a>
## Using CSS Preprocessors

<a name="keep-nesting-to-3-levels-deep-4-absolute-max"></a>
### Keep nesting to 3 levels deep, 4 absolute max.

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


<a name="style-documentation"></a>
## Style Documentation

We use multiple strategies to make sure the CSS library is understandable and maintainable:

* SMACSS prefixes indicate different types of objects.
* BEM classnames make structure clear on both the HTML and CSS sides.
* Components and other classes are documented in specially formatted comments directly in the CSS/SASS files (see sample below).
* We use [Hologram](trulia.github.io/hologram/) to auto-generate a [live style guide](//M3-SERVER/music/style_guide) from these comments, which provides a browsable inventory of components, their variants, instructions on how to use them and what they should look like.

```css
/*doc
---
title: Layout Grids
name: grid
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

