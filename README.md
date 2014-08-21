**Notice: this project is not yet release ready and under lots of flux**

Schism
======

Schism is an attempt to bring modularity to CSS - using SCSS, naming schemes and a set of rules for creating components. It encourages you to avoid many of CSS main features: "smart" selectors, the cascade and specificity (if you can call specificity a feature).

It is a framework/methodology and aimed at larger projects, where maintainability is a prime concern. Separation is encouraged over DRY, composition over inheritance.

As such, it might be a bit verbose for smaller projects.

1. [Structure](https://github.com/dpiatek/schism#structure)
  1. [Components concepts](https://github.com/dpiatek/schism#components-concepts)
    1. [Grouped components](https://github.com/dpiatek/schism#grouped-components)
    2. [Containers](https://github.com/dpiatek/schism#containers)
2. [Components](https://github.com/dpiatek/schism#components)
3. [Rules](https://github.com/dpiatek/schism#rules)

## Structure

A Schism project divides your CSS into Components, Helpers and Base. CSS and SCSS have both different abstractions for things that won't fit into these - like keyframes for animations or mixins in SCSS. These should live in their own folders and should not affect framework components in a unpredictable ways, but be available for usage (ie. don't put classes, elements selectors and similar outside of Components, Helpers and Base).

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
style.scss
```

### Components concepts
Schism provides mixins to create components. But without the right approach, components will be nothing more then your usual CSS soup.

Components should:
- be one per file
- follow the [Single responsiblity principle](http://en.wikipedia.org/wiki/Single_responsibility_principle)
- not be universal; prefer multiple components over a single complex one - this will help remove CSS rules that ensure a component behaves consistently when it's configured differently (ie. changes it's `position` from `static` to `absolute` and the surrounding HTML stop affecting the dimesions)
- not have knowledge of other components, helpers, classes, id's
- use only element and pseudo selectors
- not create complex descending selectors - create [Fragments]() instead
- be grouped if they are part of a more complex ui element (ie. navigation, modal, tabs)

Components will sometimes fall into two special categories: Grouped and Containers.

#### Grouped components

A complex ui element should be split into components and grouped together into a folder with an `_index.scss` file, which imports the components and sets variables, mixins, functions etc for the whole group.

Example structure:
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

#### Containers

Containers are layout; components which group other components in the ui. In Schism, they do not have special mixin, but it's important to separate them out conceptally when thinking about your UI. Containers will provide boundaries for your components, but content or interactive will never be a part of them - it will always be their descendant.


## Components
## Rules
