# CSS Style Guide

_Coding standards for stylesheets_

Related: **[Live Style Guide](https://m3a.vhlcentral.com/music/style_guide/)** (Components) |  **[Wiki](https://github.com/vhl/music/wiki)** (Concepts)

<a name="introduction"></a>
## Introduction

The purpose of this document is to provide guidelines for writing CSS. Code conventions are important for the long-term maintainability of code. Most of the time, developers are maintaining code, either their own or someone else’s. The goal is to have everyone’s code look the same, which allows any developer to easily work on another developer’s code.


<!-- MarkdownTOC -->

- [Coding Style](#coding-style)
  - [Indentation](#indentation)
  - [Brace Alignment](#brace-alignment)
  - [Selectors](#selectors)
  - [Properties](#properties)
  - [Vendor-Prefixed Properties](#vendor-prefixed-properties)
  - [Using CSS Preprocessors](#using-css-preprocessors)
  - [Units](#units)
    - [Do not use units with zero values](#do-not-use-units-with-zero-values)
  - [HEX values](#hex-values)
  - [String Literals](#string-literals)
  - [URLs](#urls)
  - [Attribute values in selectors](#attribute-values-in-selectors)
  - [Internet Explorer Hacks](#internet-explorer-hacks)
  - [JavaScript and Test Dependence](#javascript-and-test-dependence)
  - [:hover and :focus](#hover-and-focus)
  - [Comments](#comments)
- [Classname Conventions](#classname-conventions)
  - [Use SMACSS prefixes to distinguish classes with different roles.](#use-smacss-prefixes-to-distinguish-classes-with-different-roles)
    - [c- Component](#c--component)
    - [o- Object](#o--object)
    - [is-, has- States](#is--has--states)
    - [t- Theme](#t--theme)
    - [u- utility](#u--utility)
    - [ns- Namespace.](#ns--namespace)
    - [_ a Hack.](#_-a-hack)
    - [js- Javascript.](#js--javascript)
    - [test- Test.](#test--test)
    - [s- Scope.](#s--scope)
  - [Use BEM to reflect Component structure](#use-bem-to-reflect-component-structure)
    - [Use Hyphens between words in a block classname](#use-hyphens-between-words-in-a-block-classname)
    - [Blocks](#blocks)
    - [Elements](#elements)
    - [Modifiers](#modifiers)
  - [Multiple Classes on an Element](#multiple-classes-on-an-element)
- [Style Documentation](#style-documentation)

<!-- /MarkdownTOC -->



<a name="coding-style"></a>
## Coding Style


<a name="indentation"></a>
### Indentation

Each indentation level is made up of two spaces. Do not use tabs. (Please set your editor to use two spaces)

```css
/* Good */
.c-message {
    color: #fff;
    background-color: #000;
}

/* Bad - all on one line */
.c-message {color: #fff; background-color: #000;}
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

The opening brace should be on the same line as the last selector in the rule and should be preceded by a space. The closing brace should be on its own line after the last property and be indented to the same level as the line on which the opening brace is.

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

IMPORTANT: **Never use id's in  selectors**. They have very high specificity compared to classes, and can't be used more than once on the same page.



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



<a name="vendor-prefixed-properties"></a>
### Vendor-Prefixed Properties

When using vendor-prefixed properties, always use the standard property as well. The standard property must always come after all of the vendor-prefixed versions of the same property (This applies to vendor-prefixed property _values_ as well).

Vendor-prefixed classes should align to the left with all other properties.

```css
/* Good */
.c-directory {
  -webkit-border-radius: 4px;
  -moz-border-radius: 4px;
  transition: top 0.5s;
}

/* Bad - colons aligned */
.c-directory {
  -webkit-border-radius:4px;
     -moz-border-radius:4px;
          border-radius:4px;
}
```

Suffix property value pairs that apply only to a particular browser or class of browsers with a comment listing browsers affected.

```css
background: #fcfcfc; /* Old browsers */
background: -moz-linear-gradient(...); /* FF3.6+ */
background: -webkit-gradient(...); /* Chrome,Safari4+ */
background: -webkit-linear-gradient(...); /* Chrome10+,Safari5.1+ */
background: -o-linear-gradient(...); /* Opera 11.10+ */
background: -ms-linear-gradient(...); /* IE10+ */
background: linear-gradient(...); /* W3C */
```

Suffix fallback with “Old browsers” and standard property with “W3C”. Add a plus or minus to indicate that a property applies to all previous browsers by the same vendor or all future browsers by the same vendor.
Using !important

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

Keep nesting to 2 levels deep, 3 absolute max.

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

Multiple classes:

Extending classes: favor multiple classes over SASS @extend. Only use @mixins if they are parameterized. Otherwise it results in CSS code bloat.

```HTML
<div class="o-module  c-strand-list  u-closer"> (two spaces between each class)
```


When `@extend` _is_ used, declare it _before_ other properties:

```css
/* Bad */
.c-stubbornella {
    color: #555;
    @extend .company;
    padding: 2rem;
}

/* Good */
.c-stubbornella {
    @extend .c-company;
    color: #555;
    padding: 2rem;
}
<div class="stubbornella">...</div>

/* Better -- uses multi-class inheritance. */
.c-stubbornella {
    color: #555;
    padding: 2rem;
}
<div class="c-company  c-stubbornella">...</div>
```



<a name="units"></a>
### Units

In general, use mod() -- this is a custom SASS function that provides measures relative to a common standard. The value of mod(1) is 1rem. Rems are like ems, but without cascading problems: if a rem = 16px, you can count on that value anywhere in the page.

In order to keep things in sensible proportions, as well as improve readability, write mod values in whole numbers or fractions, but stick to halves and quarters.

```css
/* Good */
margin: mod(1);
padding: mod(1/2);

/* Bad: uses rems directly */
margin: 0.75rem;
padding: 1rem;

/* Bad: uses non-simple fractions */
margin: mod(7/8);  /* too granular */
padding: mod(3/5); /* out of proportion */

```

mod() should be used for all margins, padding, gutters, font sizes, etc.

For borders or little tweaky things like box-shadow offsets or `margin: -1px` are fine in pixels.

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

Strings should always use double quotes (never single quotes).

```css
/* Good */
.c-story__credit:after {
    content: "Stubbornella";
}

/* Bad - single quotes */
.c-story__credit:after {
    content: 'Stubbornella';
}
```


<a name="urls"></a>
### URLs

When using a url() value, always use double quotes around the actual URL.

```css
/* Good */
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
input[type="submit"] {
  ...
}

/* Bad - missing quotes */
input[type=submit] {
  ...
}

/* Bad - using single quote */
input[type='submit'] {
  ...
}
```


<a name="internet-explorer-hacks"></a>
### Internet Explorer Hacks

Only property hacks are allowed. To target Internet Explorer, use Internet Explorer-specific hacks like * and _ in the normal CSS files. Browser specific styles should not be in separate per-browser stylesheets. We prefer to keep all the CSS for a particular object in one place as it is more maintainable. In addition selector hacks should not be used. Classes like .ie6 increase specificity. Hacks should be kept within the CSS rule they affect and only property hacks should be used.

```css
/* Good */
.c-assignment {
   margin: 0;
   _margin: -1px;
}

/* Bad - uses selector hacks */
.c-assignment {
   margin: 0px;
}
.ie6 .c-assignment {
   margin: -1px;
}
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

<a name="classname-conventions"></a>
## Classname Conventions

<a name="use-smacss-prefixes-to-distinguish-classes-with-different-roles"></a>
### Use SMACSS prefixes to distinguish classes with different roles.

**SMACSS** (Scalable and Modular Architecture for CSS) advocates grouping different types
of classes based on their function. The following is a slightly modified version:

All classnames should have a prefix. Prefixes help insulate new classnames from legacy classnames. The different prefixes are usually a single letter, and are explained in the following section. 

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


<a name="o--object"></a>
#### o- Object

These are a kind of base class that gets re-used
by components. DANGER: do not modify these unless you really
know what you're doing. Could cause all kinds of mischief.
They are usually based on minor *visual* patterns in the design
(e.g., media object = "float" an image right without wrapping text)

<a name="is--has--states"></a>
#### is-, has- States

Specifies that the object or components currently is in a
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


<a name="_-a-hack"></a>
#### _ a Hack.

Used sparingly for cross-browser compatibility. Example:

```CSS
._media {zoom:1} /* Gives element 'has-layout' in IE */
```

<a name="js--javascript"></a>
#### js- Javascript.

Used for scripting hooks only. Scripts should not make reference to any other classes.

<a name="test--test"></a>
#### test- Test.

Used for automated testing hooks only. Test scripts should not make reference to any other classes.

<a name="s--scope"></a>
#### s- Scope.

Resets all the styles for a given context, by brute force. Would only be used in very specific scenarios. We're not using this currently.







<a name="use-bem-to-reflect-component-structure"></a>
### Use BEM to reflect Component structure

BEM convention (Block, Element, Modifier) uses different delimiters to make it easier to understand the role of an element:


<a name="use-hyphens-between-words-in-a-block-classname"></a>
#### Use Hyphens between words in a block classname

```css
/* Good */
.c-data-table

/* Bad - uses single underscore. */
.c_data_table

/* Bad - uses camel-case. */
.c-listInlineBoxy
```




<a name="blocks"></a>
#### Blocks

A **Block** (The parent element in an object or component) uses hyphens between words. [Prefix all class names](#use-smacss-prefixes-to-distinguish-classes-with-different-roles).

We usually refer to these as **base classes**.

```css
/* Good */
.o-list-inline
.o-list-inline__list-item
.c-list-inline--boxy

/* Bad - uses camel case*/
.u-pullLeft

/* Bad - uses single underscores */
.c-list_inline
```



<a name="elements"></a>
#### Elements

Elements are child elements of a component. They add a suffix to the base name, separated by double underscores.

```css
/* Good: double underscore */
.c-list-inline__item

/* Bad - looks like a block. */
.c-list-inline-item
```



<a name="modifiers"></a>
#### Modifiers

Modifiers are subclasses, or **variants**, of a component. They add a suffix to the base name, separated by double hyphens.

```css
/* Good: double hyphen */
.c-list-inline--boxy

/* Bad - looks like a block. */
.c-list-inline-boxy
```

When extending a component and styling the inner elements, use the base component's inner elements' class name for styling, instead of extending the class names of the inner elements as well.

Use a descendant selector (preferably child '>' ) with the modified base classname.

```css

/* Good - modifiers (subclasses) of components refer to unmodified inner element's names */
.c-list-inline--boxy > .o-list-inline__list-item

/* Bad - don't modify inner element's names */
.c-list-inline--boxy > .c-list-inline__list-item--boxy {}
.c-list-inline--boxy > .c-list-inline--boxy__list-item {}
```



<a name="multiple-classes-on-an-element"></a>
### Multiple Classes on an Element

In HTML, order classnames in a class attribute by order of object inheritance -- order in the class attribute does not matter functionally, but helps make the inheritance chain more apparent. 

Separate multiple classes with TWO spaces for readability:

```html
<!-- Good -->
<div class="o-inline-list  c-tutorial-nav  u-pull-left">

<!-- Bad - don't use single space -->
<div class="o-inline-list c-tutorial u-pull-left">
```

The classes should follow this order:

```html
<div class="[object]  [base]  [modifier  [modifier]]  [state]  [utility]  [javascript hooks]  [test hooks]">
```

This is by convention--functionality, in contrast to CSS, order of classes in HTML doesn't matter.




<a name="style-documentation"></a>
## Style Documentation

We use multiple strategies to make sure the CSS library is understandable and maintainable:

* SMACSS prefixes indicate different types of objects.
* BEM classnames make structure clear on both the HTML and CSS sides.
* Objects and other classes are documented in specially formatted comments directly in the CSS/SASS files (see sample below).
* We use [Hologram](trulia.github.io/hologram/) to auto-generate a [live style guide](//M3-SERVER/music/style_guide) from these comments, which provides a browsable inventory of components, their variants, instructions on how to use them and what they should look like.

```css


/*doc
---
title: Layout Grids
name: grid
category: Layout
---

_The body text is ideal for more detailed explanations and
documentation. It should include example HTML for the classes in question._

_Be sure to indicate what class(es) are extended by this class:_

Extends `ur-grid`

Extends `<a href>`


_**NOTE**: The example code should restart its indentation all the way to the left in order to
display properly in the style guide (the enclosing ticks are indented here for illustration):_

    ```html_example
<div class="grid">
  <div class="col-8">...</div>
  <div class="col-4">...</div>
</div>
    ```

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
/*doc
---
title: Inner Grid
name: grid-inner
parent: grid
---
*/
```




