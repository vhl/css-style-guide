# CSS Style Guide

_Coding standards for stylesheets_

Audience: non-CSS developers writing HTML.

Related: **[Live Style Guide](https://m3a.vhlcentral.com/music/style_guide/)** (Components) |  **[Wiki](https://github.com/vhl/music/wiki)** (Concepts)

## Introduction

The purpose of this document is to provide guidelines for writing CSS. Code conventions are important for the long-term maintainability of code. Most of the time, developers are maintaining code, either their own or someone else’s. The goal is to have everyone’s code look the same, which allows any developer to easily work on another developer’s code.

----

Table of Contents:

<!-- MarkdownTOC -->

- [Coding Style](#coding-style)
  - [Indentation](#indentation)
  - [Use Hyphens between words in a classname](#use-hyphens-between-words-in-a-classname)
  - [Brace Alignment](#brace-alignment)
  - [Selectors](#selectors)
    - [One selector per line](#one-selector-per-line)
  - [One Property per Line](#one-property-per-line)
  - [No Vendor-Prefixed Properties](#no-vendor-prefixed-properties)
  - [Browser workarounds](#browser-workarounds)
  - [Using !important](#using-important)
  - [Separate multiple classes with TWO spaces for readability](#separate-multiple-classes-with-two-spaces-for-readability)
  - [Units](#units)
    - [Do not use units with zero values](#do-not-use-units-with-zero-values)
  - [HEX values](#hex-values)
  - [String Literals](#string-literals)
  - [JavaScript and Test Dependence](#javascript-and-test-dependence)
  - [:hover and :focus](#hover-and-focus)
  - [Comments](#comments)
  - [Order of classes in HTML](#order-of-classes-in-html)

<!-- /MarkdownTOC -->



<a id="coding-style"></a>
## Coding Style

<a id="indentation"></a>
### Indentation

Each indentation level is made up of two spaces. Do not use tabs. (Please set your editor to use two spaces.)

```css
/* Good */
.c-message {
  color: $green;
  background-color: $white;
}

/* Bad - all on one line */
.c-message {color: $green; background-color: $white;}
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

<a id="use-hyphens-between-words-in-a-classname"></a>
### Use Hyphens between words in a classname

```css
/* Good */
.c-data-table {}

/* Bad - uses single underscore. */
.c_data_table {}

/* Bad - uses camel-case. */
.c-listInlineBoxy {}
```


<a id="brace-alignment"></a>
### Brace Alignment

The opening brace should be on the same line as the last selector in the rule and should be preceded by a space. The closing brace should be on its own line after the last property and be indented to the same level as the line with the opening brace.

```css
/* Good */
.c-description {
  color: $white;
}

/* Bad - closing brace is in the wrong place */
.c-description {
  color: $white;
  }

/* Bad - opening brace missing space */
.c-description{
  color: $white;
}
```


<a id="selectors"></a>
### Selectors

<a id="one-selector-per-line"></a>
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



<a id="one-property-per-line"></a>
### One Property per Line

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


<a id="no-vendor-prefixed-properties"></a>
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


<a id="browser-workarounds"></a>
### Browser workarounds

Suffix property value pairs that apply only to a particular browser or class of browsers with a comment listing browsers affected.

```css
background: #fcfcfc; /* Old browsers */
background: url('fills/texture-1.jpg') /* IE 8- */
background: url('fills/texture-1.jpg'), url('fills/texture-2.png'); /* W3C */
```

Suffix fallback with “Old browsers” and standard property with “W3C”. Add a plus or minus to indicate that a property applies to all previous browsers by the same vendor or all future browsers by the same vendor.


<a id="using-important"></a>
### Using !important

Do not use !important on CSS properties. The only time this is allowed is in a u- utility style.

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


<a id="separate-multiple-classes-with-two-spaces-for-readability"></a>
### Separate multiple classes with TWO spaces for readability

```html
<!-- Good -->
<div class="c-inline-list  c-tutorial-nav  u-pull-left">

<!-- Bad - don't use single space -->
<div class="c-inline-list c-tutorial-nav u-pull-left">

<!-- Bad - not ordered by object inheritance -->
<div class="c-tutorial-nav  u-pull-left  o-inline-list">
```


<a id="units"></a>
### Units

In general, use `mod()` -- this is a custom SASS function that provides measures relative to a common standard. The value of `mod(1)` is `1rem`. Rems are like ems, but without cascading problems: if a rem = 16px, you can count on that value anywhere in the page.

If it makes more sense to specify lengths in pixels, use `rpx()` -- this function abstracts pixels into fractional rems (assuming `1rem` = `16px`) and like `rem` and `mod()`, makes sure if the user changes the font size in their browser settings, everything will scale.

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

`mod()` should be used for all margins, padding, gutters, font sizes, etc.

Ems should be used very rarely -- example: if you need to add a bit of simulated small-caps inside a heading. We don't care which heading level it is, so we specify it relative to the parent with ems. You wouldn't want to use them on an element that has further levels of hierarchy.


<a id="do-not-use-units-with-zero-values"></a>
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


<a id="hex-values"></a>
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


<a id="string-literals"></a>
### String Literals

Strings should always use single quotes (never double quotes). This includes URL values.

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


<a id="javascript-and-test-dependence"></a>
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


<a id="hover-and-focus"></a>
### :hover and :focus

If :hover pseudo class is styled, :focus should also be styled for accessibility. Focus styles should never be removed entirely from any focusable element.

NOTE -- there are default focus styles in our library which should already cover most components.

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


<a id="comments"></a>
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


<a id="order-of-classes-in-html"></a>
### Order of classes in HTML

Classes should generally go in this order:

Base class > modifiers > states > utilities > js hooks > test hooks

```html
<div class="c-strand  c-strand--toc">
<div class="c-flashcard  c-flashcard--speech-rec  is-flipped">
<div class="c-module  c-module--dashboard  is-selected  u-pad-0  js-module  test-calendar">
```
