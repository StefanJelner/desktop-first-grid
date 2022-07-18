# Desktop first grid

A grid system provides the ability to present, align and order boxes of content - related to as columns in the following text - according to a predefined grid. The adavantage to "pixel perfect" approaches is, that content "snaps in" more losely and therefore discussions about single pixels can be avoided. Besides it is easier to present content in different orders and positions according to different breakpoints, respectively screen widths and orientations.

Nowaydays popular grid systems follow the mobile first approach, which means that the whole design is based on mobile devices and then from those very small screen resolutions things are rearranged and repositioned from smaller to bigger widths. This approach usually has the advantage that UIs are more tidied up and space is used more efficiently, because if the design is done the other way around and designed for desktop devices and from bigger to smaller screens, the tendency to "bloat" the UI and then running into trouble presenting things on smaller devices is higher; in the worst case, features are not available on smaller devices, which makes parts of the website or application unaccessible to smaller devices.

It all depends on the purpose of the website and application and on the target audience. If you are dealing with a big application which will mainly be used in desktop scenarios by professionals, then the desktop first approach might be the better and more time and cost effective way. This is the reason why this desktop first grid exists. It tries to provide the bare necessities for using a grid in a desktop first application. This means: breakpoints, orientation, dimensions, offsets, margins, paddings, ordering, alignment, line breaking and display.

## Importing and usage

```scss
// configuration comes here...

@import 'desktop-first-grid/desktop-first-grid';
```

## Configuration

### `$vendor`

It is possible to prefix all CSS custom properties and generated class names with a vendor prefix, which consists of the prefix and a hyphen. If an empty string is provided then no prefixing will be done.

Example:

```scss
$vendor: 'foo';
```

See https://developer.mozilla.org/en-US/docs/Web/CSS/--*

### `$grid-breakpoints`

This is a Sass map consisting of the breakpoints which will be used as key value pairs. The key is the breakpoint name and the value is the viewport width in pixel. The direction of breakpoints is from the wider to the narrower.

Example:

```scss
$grid-breakpoints: (
  xxl: 1920px,
  xl: 1440px,
  lg: 1200px,
  md: 992px,
  sm: 768px,
  xs: 576px,
);
```

See https://sass-lang.com/documentation/values/maps

### `$grid-columns`

This is the maximum amount of possible columns. It should usually be a value with a maximum number of divisors, to make equally proportioned dimensions possible, like `50%`, `33%`, `25%` aso. Usually the numbers `12` (`1`, `2`, `3`, `4`, `6`, `12`) or `24` (`1`, `2`, `3`, `4`, `6`, `8`, `12`, `24`) are used, but also `10` (`1`, `2`, `5`, `10`), `16` (`1`, `2`, `4`, `8`, `16`) or `20` (`1`, `2`, `5`, `10`, `20`) were observed in the wild.

Example:

```scss
$grid-columns: 12;
```

### `$grid-gutter-width`

The grid gutter width is the width of the gap (respectively padding) between two grid columns inside of the column itself. It is also possible to have rows without gaps between the columns, which will be shown later. Additionally with the spacing classes it is possible to override the gap, which will also be shown later.

Example:

```scss
$grid-gutter-width: 20px;
```

### `$displays`

This is a list of display properties, which will be transformed into classes. How these classes are used will be shown later.

Example:

```scss
$displays: none, inline, inline-block, block, table, table-row, table-cell, flex, inline-flex, grid;
```

### `$spacing-iterations`

This number determines, how many spacing steps (iterations) will be generated. F.ex. a value of `5` with a margin and padding base value of `5px` will result in 5 classes each with a step of `5px` and the fifth iteration will have an amount of `25px`.

Example:

```scss
$spacing-iterations: 5;
```

with a margin and padding base of `5px` will result in:

```
.mrg-1 => 5px
.pad-1 => 5px
.mrg-2 => 10px
.pad-2 => 10px
.mrg-3 => 15px
.pad-3 => 15px
.mrg-4 => 20px
.pad-4 => 20px
.mrg-5 => 25px
.pad-5 => 25px
```

Details on how these classes are used will be shown later.

### `$margin-base`

This is the step the margin iterations take.

Example:

```scss
$margin-base: 5px;
```

### `$padding-base`

This is the step the padding iterations take.

Example:

```scss
$padding-base: 5px;
```

## Breakpoints

A breakpoint is a specific screen width at which a change in representation is assumed, f.ex. because a different device with a different screen is assumed.

### `media-breakpoint-down`

This Sass mixin gives the ability to define CSS for a specific breakpoint.

Example:

```scss
.foo {
    color: red;

    @include media-breakpoint-down(xxl) {
        color: blue;
    }

    @include media-breakpoint-down(xl) {
        color: green;
    }
}
```

In the above example the default is, that the text color is `red`. Below the `xxl` breakpoint (`1920px` in our example configuration) the color will change to `blue` and below the `xl` breakpoint (`1440px` in our example configuration) the color will be `green`.

### `add-breakpoint-change`

This Sass mixin adds a `::before` pseudo element to the `body` which contains the current breakpoint as a string in the `content`-property, so it can be consumed by JavaScript on resizing of the viewport, to detect breakpoint changes.

Example:

Sass:
```scss
@include add-breakpoint-change();
```

JavaScript:
```js
window.addEventListener('resize', function() {
    var breakpoint = window.getComputedStyle(document.body, ':before').getPropertyValue('content').replace(/["']/g, '');

    console.log(breakpoint);
});
```

**Note! In a real life scenario, the `resize` events callback invocations should be audited, to prevent too many costy invocations.**

See:

- https://developer.mozilla.org/en-US/docs/Web/API/EventTarget/addEventListener
- https://developer.mozilla.org/en-US/docs/Web/API/Window/resize_event
- https://developer.mozilla.org/en-US/docs/Web/API/Window/getComputedStyle

**Note! Nowadays it is recommended to use the new `ResizeObserver` to detect the resizing of DOM elements. See: https://developer.mozilla.org/en-US/docs/Web/API/ResizeObserver**

## Display

Display classes work analogous to the real `display` values in CSS.

### `add-display-classes`

This mixin creates the display classes.

Example:

Sass:
```scss
@include add-display-classes();
```

HTML:
```html
<div class="d-block"></div>

<div class="d-inline-block"></div>

<div class="d-flex d-xl-none"></div>
```

The third example shows, how classes can be breakpoint dependent; the default `display` is `flex`, but below the `xl` breakpoint (`1440px` in our example configuration) the `display` will be `none`, which will render the element invisible.

## Flex

Flex classes work analogous to the real `flex` related properties and values in CSS.

### `add-flex-classes`

This mixin creates the display classes.

Example:

Sass:
```scss
@include add-flex-classes();
```

HTML:
```html
<div class="flex-row"></div>
<div class="flex-column"></div>
<div class="flex-row-reverse"></div>
<div class="flex-column-reverse"></div>
<div class="flex-row flex-xl-column flex-sm-column-reverse"></div>

<div class="flex-grow-0"></div>
<div class="flex-grow-1"></div>
<div class="flex-grow-1 flex-xl-grow-0"></div>

<div class="flex-shrink-0"></div>
<div class="flex-shrink-1"></div>
<div class="flex-shrink-1 flex-xl-shrink-0"></div>

<div class="flex-wrap flex-xl-nowrap flex-sm-wrap-reverse"></div>

<div class="justify-content-start justify-content-xl-center"></div>

<div class="align-items-start align-items-xl-center"></div>

<div class="align-items-start align-items-xl-center"></div>

<div class="align-content-start align-content-xl-center"></div>

<div class="order-1 order-xl-3"></div>
<div class="order-2"></div>
<div class="order-3 order-xl-1"></div>
```

## Spacing

Spacing functions and classes work analogous to the real `margin` and `padding` related properties and values in CSS.

### `margin`

This function returns a multiple of the `$margin-base` configuration as a `calc()` expression.

```scss
@debug margin(1); // outputs 'var(--margin-base)'

@debug margin(2); // outputs 'calc(2 * var(--margin-base))'
```

See https://developer.mozilla.org/en-US/docs/Web/CSS/calc

### `padding`

This function returns a multiple of the `$padding-base` configuration as a `calc()` expression.

```scss
@debug padding(1); // outputs 'var(--padding-base)'

@debug padding(2); // outputs 'calc(2 * var(--padding-base))'
```

See https://developer.mozilla.org/en-US/docs/Web/CSS/calc

### `add-spacing-classes`

This mixin creates the spacing classes.

Syntax:
```
mrg[-$breakpoint][-$direction]-$step
mrg[-$breakpoint][-$direction]-auto
pad[-$breakpoint][-$direction]-$step
```

Sass:
```scss
@include add-spacing-classes();
```

HTML:
```html
<div class="mrg-2 mrg-xxl-3 mrg-xl-2 mrg-sm-0 mrg-xs-auto"></div>
<div class="pad-2 pad-xxl-3 pad-xl-2 pad-sm-0"></div>

<div class="mrg-t-2 mrg-xxl-t-3 mrg-xl-t-2 mrg-sm-t-0 mrg-xs-t-auto"></div>
<div class="pad-t-2 pad-xxl-t-3 pad-xl-t-2 pad-sm-t-0"></div>

<div class="mrg-tb-2 mrg-xxl-tb-3 mrg-xl-tb-2 mrg-sm-tb-0 mrg-xs-tb-auto"></div>
<div class="pad-tb-2 pad-xxl-tb-3 pad-xl-tb-2 pad-sm-tb-0"></div>

<div class="mrg-lr-2 mrg-xxl-lr-3 mrg-xl-lr-2 mrg-sm-lr-0 mrg-xs-lr-auto"></div>
<div class="pad-lr-2 pad-xxl-lr-3 pad-xl-lr-2 pad-sm-lr-0"></div>
```

`mrg` is the shorthand for `margin` and `pad` for `padding`. The following shorthands are used for the directions:

- `l` for `left`
- `r` for `right`
- `lr` for `left` and `right`
- `t` for `top`
- `b` for `bottom`
- `tb` for `top` and `bottom`

If no direction is provided, then the `margin` or `padding` is added in all directions.

The step values are between `0` and the `$spacing-iterations` configuration. Additionally `auto` is possible.

## Grid

It is important to understand that the term *grid* does not relate to the `display` value `grid`, which is part of the CSS grid specification. What is meant here is more like a flexible - flexbox based - grid.

### `add-grid-classes`

This mixin creates the grid classes.

Sass:
```scss
$grid-columns: 12;

@include add-spacing-classes();
```

HTML:
```html
<div class="container">
    <div class="row">
        <div class="col">Content 1</div>
        <div class="col">Content 2</div>
    </div>
</div>
```

This shows 2 columns with the same `width` of `50%`.

HTML:
```html
<div class="container">
    <div class="row">
        <div class="col-6">Content 1</div>
        <div class="col-6">Content 2</div>
    </div>
</div>
```

This shows 2 columns with the same `width` of `50%`.

HTML:
```html
<div class="container-fluid">
    <div class="row no-gutters">
        <div class="col-4">Content 1</div>
        <div class="col-4">Content 2</div>
        <div class="col-4">Content 3</div>
    </div>
</div>
```

This shows 3 columns with the same `width` of `33.33%`. Additionally `container-fluid` gives the container a `width` of `100%` and `no-gutters` prevents any gap between the columns.

HTML:
```html
<div class="container">
    <div class="row">
        <div class="col-4 col-md-6">Content 1</div>
        <div class="col-4 col-md-6">Content 2</div>
        <div class="d-none d-md-block flex-md-break"></div>
        <div class="col-4 col-md-12">Content 3</div>
    </div>
</div>
```

This shows 3 columns with the same `width` of `33.33%` and below the `md` breakpoint (`992px` in our example configuration) it shows a row with 2 columns with a `width` of `50%` and a row with 1 column with a `width` of `100%`.

HTML:
```html
<div class="container">
    <div class="row">
        <div class="col-6 col-md-auto">Content 1</div>
        <div class="col-6 col-md-auto">Content 2</div>
        <div class="d-none d-md-block col-md-auto">Content 3</div>
    </div>
</div>
```

This shows 2 columns with the same `width` of `50%` and below the `md` breakpoint (`992px` in our example configuration) it shows 3 columns with an equal `width` of `33.33%`.

HTML:
```html
<div class="container-fluid">
    <div class="row">
        <div class="col">Content 1</div>
        <div class="col-min">Content 2</div>
        <div class="col">Content 3</div>
    </div>
</div>
```

This shows a container with a `width` of `100%` and inside of it 3 columns and the columns 1 and 3 have the same width, while the column 2 is only as wide as its content.

HTML:
```html
<div class="container-fluid">
    <div class="row">
        <div class="col flex-grow-1">Content 1</div>
        <div class="col flex-grow-0">Content 2</div>
    </div>
</div>
```

This shows a container with a `width` of `100%` and inside of it 2 columns and the column 1 takes as much space as possible.

HTML:
```html
<div class="container-fluid">
    <div class="row">
        <div class="col">Content 1</div>
        <div class="col pad-r-0">Content 2</div>
        <div class="col pad-l-0">Content 3</div>
    </div>
</div>
```

This shows a container with a `width` of `100%` and inside of it 3 columns and the columns 2 and 3 have no gap between them.

HTML:
```html
<div class="container-fluid">
    <div class="row">
        <div class="col-4">Content 1</div>
        <div class="col-2 offset-2">Content 2</div>
        <div class="col-4">Content 3</div>
    </div>
</div>
```

This shows a container with a `width` of `100%` and inside of it 3 columns and the column 2 has a left offset of `16.66%`.

## Misc

## `vendor`

This function returns the configured vendor prefix with an additional hyphen or an empty string, if no vendor prefix is set (which means it is an empty string). This is usually used internally as a convenience function.

Example:

```scss
$vendor: 'foo';

@debug vendor(); // outputs 'foo-'

$vendor: '';

@debug vendor(); // outputs ''
```

## `negative`

This function takes a literal CSS value and returns a `calc()` expression which negates the literal CSS value.

Example:

```scss
@debug negative(1px); // outputs 'calc(-1 * 1px)'

@debug negative(var(--foo)); // outputs 'calc(-1 * var(--foo))'

$foo: 1px;

@debug negative(#{$foo}); // outputs 'calc(-1 * 1px)'
```

**Note! If a Sass variable should be used, it is important to provide it as a literal value, surrounded by `#{}`.**

See https://developer.mozilla.org/en-US/docs/Web/CSS/calc
