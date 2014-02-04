# LESS Styleguide
A set of rules for us to follow so our stylesheets as minimally
headache-inducing as possible. You should be familiar with CSS specificity rules
and the features of (LESS)[http://lesscss.org/features/] to understand this guide.

## Coding Style

* Use soft-tabs with a two space indent.
* Put spaces after `:` in property declarations.
* Put spaces before `{` in rule declarations.
* Use hex color codes `#000` unless using rgba.
* Use `//` for comment blocks (instead of `/* */`).

Here is good example syntax:

```
// This is a good example!
.styleguide-format {
  border: 1px solid #0f0;
  color: #000;
  background: rgba(0,0,0,0.5);
}
```

## SCSS Style

Any `@variable` or `@mixin` that is used in more than one file should be put in
`variables.less` and any `@mixin` that is used in more than one file should be
put in `mixins.less`. Variables that are only used within one file should be placed
at the top of the scope in which it is used. This means a variable that is used
throughout a single file should be defined at the top of the file, and a
variable that is only used within a block, the variable should be defined at the
beginning of that block.

Whenever specifying child or pseudo-class relationships within
selectors, write the relationships as nested selectors. For example: 

```less
.styleguide-format {
  a {
    &:hover {
    color: #000;
    }
  }
}
``` 
is preferred to
```less
.styleguide-format a:hover {
  color: #000;
}
```
As a rule of thumb, don't nest further than 3 levels deep. If you find
yourself going further, think about reorganizing your rules (either the
specificity needed, or the layout of the nesting). 

## File Organization

In general, the CSS file organization should follow something like this:

```plain
less
├── shared
│   ├── forms.less
│   ├── listings.less
│   └── typeface.less
├── sections
│   ├── profile.less
│   └── settings.less
├── variables.less
├── mixins.less
├── bootstrap.less
├── layout.less
└── style.less
```
The directory hierarchy should be used to organize files. Smaller files
pertaining to logical units are preferred over larger files that cover multiple
components. 

When importing LESS files, drop the `.less` extension. If you are using on any
variables or mixins in a file, explicitly `@import` the files that that file
depends on. Like so:

```less
@import "../variables";

.rule { ... }
```

`bootstrap.less` should import from customized `variables.less` and
`mixins.less` files before importing the other bootstrap component files, so
that changes can propagate through the bootstrap build.

## Pixels vs. Ems

Use px for font-size, because it offers absolute control over text. Additionally, unit-less line-height is preferred because it does not inherit a percentage value of its parent element, but instead is based on a multiplier of the font-size.

## Class naming conventions

Use hyphenated class names.

## Specificity (classes vs. ids)

Elements that occur exactly once inside a page should use IDs, otherwise, use classes. When in doubt, use a class name.

* Good candidates for ids: header, footer, modal popups.
* Bad candidates for ids: navigation, item listings, item view pages (ex: issue view).

When styling a component, start with an element + class namespace (prefer class
names over ids), prefer direct descendant selectors by default, and use as
little specificity as possible. Here is a good example:

```html
<ul class="category-list">
  <li class="item">Category 1</li>
  <li class="item">Category 2</li>
  <li class="item">Category 3</li>
</ul>
```

```less
ul.category-list { // element + class namespace

  > li { // direct descendant selector > for list items
    list-style-type: disc;
  }

  a { // minimal specificity for all links
    color: #ff0000;
  }
}
```

## CSS Specificity guidelines

* If you must use an id selector (`#selector`) make sure that you have no more
  than one in your rule declaration. A rule like `#header .search #quicksearch {...}` is considered harmful.
* When modifying an existing element for a specific use, try to use specific
  class names. Instead of `.listings-layout.bigger` use rules like
  `.listings-layout.listings-bigger`. Think about `ack`/`grep`ing your code in
  the future.
