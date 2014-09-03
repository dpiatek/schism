**Notice: this project is not yet release ready and under lots of flux**

Schism
======

Schism is an attempt to bring modularity to CSS. At the core it is just a few  SCSS mixins and a methodology for building a library of components.

It is aimed at larger projects, where maintainability is a prime concern. Separation is encouraged over DRY; composition over inheritance.

1. [Components](https://github.com/dpiatek/schism#components)
-  [Structure](https://github.com/dpiatek/schism#structure)

## Components
In Schism you should follow these rules when creating components:

1. There should be only one component per file (so only one [Component]() mixin)
2. Components should adhere to the [Single responsiblity principle](http://en.wikipedia.org/wiki/Single_responsibility_principle)
3. Components should not be universal; prefer multiple components over a single complex one - this will help remove CSS rules that ensure a component behaves consistently when it's configured differently (ie. changes it's `position` from `static` to `absolute` and the surrounding HTML stop affecting the dimesions)
4. Components should not have knowledge of other components, helpers, classes or id's. You should only use pseudo-elements, pseudo-classes and element selectors.
5. Use the [Fragment]() mixin to create classes that belong to the component (but if they are complex or require hierarchy, create a new component instead)
6. Use the [Modifier]() mixin if you need a class to modify a `Component` or `Fragment`
7. Components should be [grouped]() if they are part of a more complex ui element (ie. navigation, modal, tabs)

If have to break them, use a [shame.scss](http://csswizardry.com/2013/04/shame-css/) so you can come back and try to fix it later.

Schism provides mixins that all components should be created with. The `Component` mixin defines the component and adds it to a global list.

Example:
```
@include Component(search-form) {
  // ...
}

// .search-form {
//   ...
// }
```

Components will sometimes fall into two special categories: Grouped and Containers.

### Grouped components

A complex ui element should be split into components and grouped together into a folder with an `_index.scss` file, which imports the components and sets variables, mixins, functions etc for the whole group.

Example grouped component:
```
tabs
  _index.scss
  _panel.scss
  _tab.scss
  _tabs-nav.scss
```

Example index.scss:
```scss
// Tabs

$tabs-bg-color:  #e5e5e5;
$tabs-fg-color:  #444;
$tabs-highlight: #8a00;

%tabs-spacing {
  padding: 12px 16px;
  border: 1px solid transparent;
}

@import "panel";
@import "tab";
@import "tabs-nav";
```

Sometimes, it will make sense to create a skeleton component and then use @extend to compose a new component/fragment. Schism helpers help you do that by outputting SCSS placeholders for every component and fragment you create. A good example could be building a shopping cart table which extends a simple table or right sidebar layout extending a simple padded container layout.

### Containers

Containers are layout; components which group other components in the ui. In Schism, they do not have special mixin, but it's important to separate them out conceptally when thinking about your UI. Containers will provide boundaries for your components, but content or interactive will never be a part of them - it will always be their descendant.

### Fragments

Fragments form classes bound to your Component; they are too small to be separate Components but too distinct to just be referenced with an element selector.

## Structure

A Schism project divides your CSS into Components, Helpers and Base. CSS and SCSS have both different abstractions for things that won't fit into these - like keyframes for animations or mixins in SCSS. These should live in their own folders and should not affect framework components in unpredictable ways, but be available for usage.

- **_imports.scss** - imports all partials. Correct order should be maintained - Helpers, Base, animations (and your other abstractions) should come before components. Order of components should not mather.
- **_theme.scss** - should contain your common design patterns: your brand colors, gradients, borders, shadows etc.
- **style.scss** - this is your output file.

An example project structure:
```
animations
  _flash.scss
base
  _typography.scss
components
  layout
    _blog.scss
    _index.scss
    _standard.scss
  tabs
    _index.scss
    _panel.scss
    _tab.scss
    _tabs-nav.scss
  _media.scss
  _vertical-nav.scss
  _video.scss
helpers
  _image.scss
  _placeholder.scss
mixins
  _font-size.scss
  _list-reset.scss
  _vendor.scss
vendor
  _obscure-jquery-plugin.scss
_imports.scss
_theme.scss
style.scss
```

