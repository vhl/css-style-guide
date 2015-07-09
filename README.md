# OOCSS Code Standards


<a name="introduction"></a>
## Introduction

The purpose of this document is to provide guidelines for writing CSS. Code conventions are important for the long-term maintainability of code. Most of the time, developers are maintaining code, either their own or someone else’s. The goal is to have everyone’s code look the same, which allows any developer to easily work on another developer’s code.

The architecture itself is heavily based on [OOCSS](https://github.com/stubbornella/oocss/wiki). Class naming conventions are adapted from Harry Roberts’ [More Transparent UI Code with Namespaces](http://csswizardry.com/2015/03/more-transparent-ui-code-with-namespaces).

If you're already familiar with the conceptual guidelines & the class naming standards, you can skip right to [Coding Style](#coding-style).




<!-- MarkdownTOC -->

- [Object-Oriented CSS (OOCSS)](#object-oriented-css-oocss)
  - [Separate Structure from Skin](#separate-structure-from-skin)
  - [Separate Containers from Content](#separate-containers-from-content)
    - [Component Width and Height](#component-width-and-height)
  - [Class Qualification](#class-qualification)
  - [Object Composition with Multiple Classes](#object-composition-with-multiple-classes)
  - [Avoid using IDs](#avoid-using-ids)
  - [Avoid Compound Selectors Whenever Possible](#avoid-compound-selectors-whenever-possible)
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
- [Style Documentation](#style-documentation)

<!-- /MarkdownTOC -->








<a name="object-oriented-css-oocss"></a>
## Object-Oriented CSS (OOCSS)

OOCSS advocates practices which are beneficial to flexibility, code reuse, maintenance and performance.




<a name="separate-structure-from-skin"></a>
### Separate Structure from Skin

Objects should provide structure (e.g., grid layout). They solve the hard problems that we encounter again and again.

Components and skins (modifiers) should determine more cosmetic aspects (color, font, border, background).


<a name="separate-containers-from-content"></a>
### Separate Containers from Content

A component should have no knowledge of its container.

Avoid location-based styles:

```css
/* Bad: location-based (also: ID selector!) */
.score {font-size: 1.1rem;}
#footer .score {font-size: 1.4rem;}
```



```css
/* Good */
.score {font-size: 1.1rem;}
.score--lg {font-size: 1.4rem;}
```

`.score--lg` is a modifier that is placed on the same element as `.score`.


<a name="component-width-and-height"></a>
#### Component Width and Height

* Containers (grids, modules) dictate the width of their contents.
  * Avoid setting widths on anything else.
* Content dictates the height of its container.
  * Avoid setting heights on containers.

Components should be flexible and their widths controlled by grids.

```css
/* Good - dimensions unspecified */
.c-callout {
  display: block;
  border: 1px solid #ccc;
}

/* Bad - dimensions specified */
.c-callout {
  display: block;
  width: 200px;
  height: 150px;
  border: 1px solid #ccc;
}
```




<a name="class-qualification"></a>
### Class Qualification

Do not over-qualify class name selectors with an element type unless you are specifying exceptions to the default styling of a particular class. Leaving out the element name makes the class reusable on different kinds of elements.

```css
/* Good */
.c-button-link {}

/* Bad - element name should be omitted */
span.c-button-link {}

/* Good - element is providing exceptions */
.c-button-link {}
span.c-button-link {}
```



<a name="object-composition-with-multiple-classes"></a>
### Object Composition with Multiple Classes

Order classnames in a class attribute by order of object inheritance -- order in the class attribute does not matter functionally, but helps make the inheritance chain more apparent. Separate multiple classes with TWO spaces for readability.

```html
<!-- Good -->
<div class="o-inline-list  c-tutorial-nav  u-pull-left">

<!-- Bad - don't use single space -->
<div class="o-inline-list c-tutorial u-pull-left">
```



<a name="avoid-using-ids"></a>
### Avoid using IDs

Selectors should NEVER use HTML element IDs, since they increase specificity and make it difficult to override styles. Always use classes for applying styles.

```css
/* Good */
.c-header {
   height: 100px;
}

/* Bad - using an ID */
#column_wrapper {
   height: 100px;
}
```




<a name="avoid-compound-selectors-whenever-possible"></a>
### Avoid Compound Selectors Whenever Possible

Use single, name-qualified selectors when styling component elements--don't use compound selectors, which will increase specificity.

```css
/* Good */
.c-tabset__tab {}

/* Bad - not necessary; child class is unique enough. */
.c-tabset > .c-tabset__tab
```






<a name="class-naming-conventions"></a>
## Class Naming Conventions




<a name="all-classnames-should-have-a-prefix"></a>
### All classnames should have a prefix

Prefixes help insulate BEM classes from any legacy classnames. The different prefixes are usually a single letter, and are explained in the [music gem documentation](). The system used here is adapted from [SMACSS](https://smacss.com/).

```css
/* Good */
.c-breadcrumbs

/* Bad - no prefix */
.breadcrumbs
```



<a name="use-bem-to-reflect-component-structure"></a>
### Use BEM to reflect component structure

BEM convention (Block, Element, Modifier) uses different delimiters to make it easier to understand the role of an element.



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

A **Block** (The parent element in an object or component) uses hyphens between words. Prefix all class names with the first letter of their type: o- Object, c- Component, l-Layout, u- Utility. See the [README](https://github.com/vhl/music/blob/library/app/assets/stylesheets/music/README.md) in the music gem library for more details.

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

Modifiers are subclasses, or variants, of a component. They add a suffix to the base name, separated by double hyphens.

```css
/* Good: double hyphen */
.c-list-inline--boxy

/* Bad - looks like a block. */
.c-list-inline-boxy
```

When extending a component and styling the inner elements, use the base component's inner elements' class name for styling, instead of extending the class names of the inner elements as well.

```css

/* Good - modifiers (subclasses) of components refer to unmodified inner element's names */
.c-list-inline--boxy > .o-list-inline__list-item

/* Bad - don't modify inner element's names */
.c-list-inline--boxy > .c-list-inline__list-item--boxy {}
```





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

Declare `@extend` before other properties. Keep in mind that extending via multiple classes in the markup should be preferred over SASS '@extend' in the majority of cases.

```css
/* Good */
.c-stubbornella {
    @extend .c-company;
    color: #555;
    padding: 2rem;
}
<div class="stubbornella">...</div>

/* Better */
.c-stubbornella {
    color: #555;
    padding: 2rem;
}
<div class="c-company  c-stubbornella">...</div>

/* Bad */
.c-stubbornella {
    color: #555;
    @extend .company;
    padding: 2rem;
}
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

_The long description is ideal for more detailed explanations and
documentation. It should include example HTML for the classes in question._

_Be sure to indicate what class(es) are extended by this class:_

Extends `ur-grid`

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
