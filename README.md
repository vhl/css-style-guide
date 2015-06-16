# CSS code standards

The purpose of this document is to provide guidelines for writing CSS. Code conventions are important for the long-term maintainability of code. Most of the time, developers are maintaining code, either their own or someone else’s. The goal is to have everyone’s code look the same, which allows any developer to easily work on another developer’s code.

We've borrowed some ideas from [Idiomatic CSS](https://github.com/necolas/idiomatic-css) and credited it throughout the document.

The architecture itself is based on [OOCSS](https://github.com/stubbornella/oocss/wiki). Class naming conventions are from Harry Roberts’ post on [Namespacing CSS](http://csswizardry.com/2015/03/more-transparent-ui-code-with-namespaces), incorporating aspects of [BEM](https://en.bem.info/), [SMACSS](https://smacss.com/), and [SUIT](https://github.com/suitcss/suit). 



### Class Names

BEM convention uses different delimiters to make it easier to understand the role of an element.

A **Block** (The parent element in an object or component) uses hyphens between words. Prefix all class names with the first letter of their type: o- Object, c- Component, l-Layout, u- Utility. See the [README](https://github.com/vhl/music/blob/library/app/assets/stylesheets/music/README.md) in the music gem library for more details.

```css
/* Good */
.o-list-inline {}
.o-list-inline__list-item {}
.c-list-inline--boxy {}

/* Bad - don't use unprefixed */
.list-inline

/* Bad - don't use camel case*/
.u-pullLeft {}

/* Bad - don't use single underscores */
.c-list_inline {}
```

**Elements** are subclasses, or variants, of a component. They add a suffix to the base name, separated by double hyphens.

```css
/* Good */
.c-list-inline--boxy

/* Bad - don't blur distinction between block and modifier */
.c-list-inline-boxy
```

**Modifiers** are subclasses, or variants, of a component. They add a suffix to the base name, separated by double hyphens.

```css
/* Good */
.c-list-inline--boxy

/* Bad - don't blur distinction between block and modifier */
.c-list-inline-boxy
```

When extending a component and styling the inner elements, use the base component's inner elements' class name for styling, instead of extending the class names of the inner elements as well.

```css

/* Good - modifiers (subclasses) of components refer to unmodified inner element's names */
.c-list-inline--boxy > .o-list-inline__list-item

/* Bad - don't modify inner element's names */
.c-list-inline--boxy > .c-list-inline__list-item--boxy {}
```



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

### Property Format

Each property must be on its own line and indented one level. There should be no space before the colon and one space after. All properties must end with a semicolon.

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

### Using CSS Preprocessors

Keep nesting to 2 levels deep, 3 absolute max.

```scss
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

Declare `@extend` followed by `@include` statements first in a declaration block. (Borrowed from [Idiomatic CSS] (https://github.com/necolas/idiomatic-css#4-format))

```scss
/* Good */
.stubbornella {
    @extend .company;
    @include font-size(14);
    color: #555;
    font-size: 11px;
}

/* Bad */
.stubbornella {
    color: #555;
    @extend .company;
    font-size: 11px;
    @include font-size(14);
}
```

### Vendor-Prefixed Properties

When using vendor-prefixed properties, always use the standard property as well. The standard property must always come after all of the vendor-prefixed versions of the same property.

```css
.c-directory {
    -moz-border-radius: 4px;
    -webkit-border-radius: 4px;
    border-radius: 4px;
}
```

If a vendor prefixed property is used, -moz, -webkit, -o, -ms vendor prefixes should also be used. Vendor-prefixed classes should align to the left with all other properties.

```css
/* Good */
-webkit-border-radius: 4px;
-moz-border-radius: 4px;
border-radius: 4px;

/* Bad - colons aligned */
-webkit-border-radius:4px;
   -moz-border-radius:4px;
        border-radius:4px;
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

### Font Sizing

All font sizes must be specified using rem only. Do not use percentages, ems or pixels.

```css
/* Good */
.c-story {
   font-size: 14px; /* pixel fall back rule should come first */
   font-size: 1.4rem;
}

/* Bad - uses ems */
.c-story {
   font-size: 1.2em;
}

/* Bad - uses percentage */
.c-story {
   font-size: 86%;
}

/* Bad - uses pixel only */
.c-story {
   font-size: 14px;
}
```

### HEX value

When declaring HEX values, use lowercase and shorthand (where possible) (Borrowed from [Idiomatic CSS] (https://github.com/necolas/idiomatic-css#4-format))

```css
/* Good */
.c-story__credit {
    color: #ccc;
}

/* Bad */
.c-story__credit {
    color: #CCCCCC;
}
```

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

### Background Images and Other URLs

When using a url() value, always use quotes around the actual URL. 

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

### Attribute values in selectors

Use double quotes around attribute selectors.

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


### Do not use units with zero values

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

### Class Qualification

Do not over-qualify class name selectors with an element type unless you are specifying exceptions to the default styling of a particular class.

``` css
/* Good */
.c-button-link {}

/* Bad - element name should be omitted */
span.c-button-link {}

/* Good - element is providing exceptions */
.buttonAsLink {}
span.c-button-link {}
```

### Component Elements

Use single, name-qualified selectors when styling component elements--don't use compound selectors, which will increase specificity.

```css
/* Good */
.c-tabset__tab {}

/* Bad - not necessary; child class is unique enough. */
.c-tabset > .c-tabset__tab
```

### Classnames in HTML

Order classnames in a class attribute by order of object inheritance for understandability (functionally, the order of classnames in HTML makes no difference). Separate multiple classes with TWO spaces for scannability.

```html
<!-- Good -->
<div class="c-tutorial-nav  u-pull-left">

<!-- Bad - don't use single space -->
<div class="c-tutorial u-pull-left">


### JavaScript and Test Dependence

If an item is manipulated by Javascript, it should be have a js- class purposes of selecting that element. Javascript should not query elements by any other classes.

```js
<!-- Good -->
<div class="c-tabset  js-tabset">
<script> tabset = $('.js-tabset'); </script>

<!-- Bad: -->
<div class="c-tabset">
<script> tabset = $('.c-tabset'); </script>
```

### :hover and :focus

If :hover pseudo class is styled, :focus should also be styled for accessibility. Focus styles should never be removed.

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

### Avoid using IDs

Selectors should never use HTML element IDs. Always use classes for applying styles to specific areas of a page.

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

### Width and height on components

No heights on anything that contains text. Components should be flexible and their widths should be controlled by grids.

```css
/* Good - no width specified */
.c-calloutContent {
    border: 1px solid #ccc;
    background: #fff;
}

/* Bad - dimension specified */
.c-calloutContent {
    width: 200px;
    height: 150px;
    border: 1px solid #ccc;
    background: #fff;
}
```


### Comments

```css
/* ==========================================================================
   Section comment block
   ========================================================================== */

/* Sub-section comment block
   ========================================================================== */

/* Basic one-line comment */


### Style Documentation

We use [Hologram](trulia.github.io/hologram/) to auto-generate a live style guide (/music/style_guide). Objects and other classes need to be documented in specially formatted comments directly in the CSS/SASS files:

/*doc
---
title: Layout Grids
name: grid
category: Layout
---

  The long description is ideal for more detailed explanations and
  documentation. It should include example HTML for the classes in question.
  This markup should reset its indentation all the way to the left in order to
  display properly in the style guide:

  ```html_example
<div class="grid">
  <div class="col-8"></div>
  <div class="col-4"></div>
</div>
  ```

  TODO: This is a todo statement that describes an atomic task to be completed
    at a later date. It wraps after 80 characters and following lines are
    indented by 2 spaces.

/*
