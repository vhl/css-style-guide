# CSS Style Guide

_Coding standards for stylesheets_

Audience: non-CSS developers writing HTML.

Related: **[Live Style Guide](https://m3a.vhlcentral.com/music/style_guide/)** (Components) |  **[Wiki](https://github.com/vhl/music/wiki)** (Concepts)

## Introduction

The purpose of this document is to provide guidelines for writing CSS. Code conventions are important for the long-term maintainability of code. Most of the time, developers are maintaining code, either their own or someone else’s. The goal is to have everyone’s code look the same, which allows any developer to easily work on another developer’s code.


<!-- MarkdownTOC -->

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
  - [Order of classes in HTML](#order-of-classes-in-html)
- [Adding New Feature-specific Styles](#adding-new-feature-specific-styles)

<!-- /MarkdownTOC -->



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


<a name="order-of-classes-in-html"></a>
### Order of classes in HTML

Classes should generally go in this order:

Base class > modifiers > states > utilities > js hooks > test hooks

```html
<div class="c-strand  c-strand--toc">
<div class="c-flashcard  c-flashcard--speech-rec  is-flipped">
<div class="c-module  c-module--dashboard  is-selected  u-pad-0  js-module  test-calendar">
```



<a name="adding-new-feature-specific-styles"></a>
## Adding New Feature-specific Styles

If you need to add new styles, but them in the /features directory in the music gem. Create a subfolder for your feature if one doesn't exist.

* Feature style classnames adhere to the same guidelines as any others, just make sure to NEVER give a feature class the same name as a library class (Note: the `f-` prefix has been deprecated).
 
* Adding a modifier to a class that exists in the library is encouraged: e.g., if `c-module` exists in the library, you can add a style in your "Gradebook v2" feature that defines `c-module--grade-detail`, provided that class does not already exist in the library. 

* For features using the existing pre-v1 CSS and layouts: Import your feature stylesheet in your views, then scope your styles by adding a feature namespace class like "ns-gradebook-v2" and prefix all your selectors with it:

  * Example: `.ns-gradebook-v2 .c-student-name { ... }`

* For new music v1 features only: All feature stylesheets are included in the main stylesheet, so there will no longer be a need to import them on a per view basis.

