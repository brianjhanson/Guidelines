# CSS at ODC

This is an attempt to start nailing down our CSS methodology at ODC. I feel like we follow it pretty closely, but I wanted to open up a discussion and start tying to document it. 

- Mix of BEM and SMACSS methodology (mostly BEM for naming but with the h, l, to indicate type)

I'm taking a lot of this from [Hugo Giraudel's Sass Guidelines](http://sass-guidelin.es/) but feel it's worth writing out specifically which rules we prefer.

## Basic Syntax & Formatting
- roughly 80 character line lengths
- Multi-line CSS Rules
- Meaningful use of whitepace
- Always wrap strings in single quotes
- Numbers should display leading zeros before a decimal value less than one.
- 0 values should not include units
- To add units to a number always multiply the nubmer by 1 unit.
- Always wrap calculations in parentheses
- Avoid magic numbers when possible. Comment them when not possible.

**Example**
```css
// Yep
.foo {
  font-family: 'Helvetica Neue Light', 'Helvetica', 'Arial', sans-serif;
  opacity: 0.5;
  display: block;
  overflow: hidden;
  padding: 0 1em;
  width: (42 * 1px);
}

// Nope
.foo {
  font-family: Helvetica Neue Light, Helvetica, Arial, sans-serif;
  opacity: .5;
  display: block; overflow: hidden;

  padding: 0em 1em;
  width: (42 + px);
}
```

## Maps
- Space after the colon
- Opening brace on the same line as the colon
- Quote keys that are strings
- each key/value pair is on its own line
- comma at the end of each key/value
- trailing comma on the last line
- closing brace on its own new line
- no space or new line between closing brace and semi-colon

**Example**
```css
// Yep
$breakpoints: (
  'small': 767px,
  'medium': 992px,
  'large': 1200px,
);

// Nope
$breakpoints: ( small: 767px, medium: 992px, large: 1200px );
```

## Nesting
While nesting makes it very simple to create unnecessary specificity as well as code bloat. It's usually best to avoid it except in cases where a property is very closely related to the selector it is under (e.g. psuedo selectors and state classes). In cases where nesting is appropriate, keep it under 3 levels if possible.

## Naming Conventions
This section follows [Harry Robert's guidelines on the subject](http://cssguidelin.es/#naming-conventions)

All strings in classes are delimited with a hyphen ( - ), meaning camel case and underscores are discouraged.

**Example**
```css
// Yep
.page-head {}
.sub-content {}

// Nope
.pageHead {}
.sub_content {}
```

We prefer to use a BEM-like naming convention. BEM (Block, element, modifier) is a popular and complete methodology. We only really adhere to their naming conventions.

There are three groups of names in the BEM naming convention
- Block: The root of the comonent
- Element: A component of the block
- Modifier: a variant or extension of the block

**Example**
```css
.person {}
.person__head {}
.person--tall {}
```

Elements are delimited with 2 underscores and modifiers are delimited by two hyphens

Block context starts at the most logical, self-contained, discrete location. To continue down our person-based analogy, we would avoid a class like `.room__person {}`, as the room is in a higher context than the person. Instead, it would be better to separate them into different blocks.

**Example**
```css
// Yep
.room {}
  .room__door {}

.person {}
  .person__head {}

// Nope
.room__person {}
  .room__person__head {}
```

If we want to denote a `.person {}` inside of a `.room {}`, it would be more correct to use a selector like `.room .person {}` which bridges the space between two blocks.

Here's a more practical example
```css
// Yep
.page {}
.content {}
.sub-content {}
.footer {}
  .footer__copyright {}

// Nope
.page {}
  .page__content {}
  .page__sub-content {}
  .page__footer {}
    .page__copyright {}
```

If we add another element to our person, we do not need to step through eery layer of the DOM but rather add it to the object base. In this way objects imply dependency, not structure.

```css
// Yep
.person {}
  .person__head {}
  .person__eye {}

// Nope
.person {}
  .person__head {}
  .person__head__eye {}
```

In order to modify Elements we primarily use decendent selectors. This means if we have a tall person `.person--tall {}` we can style their legs based on their relation to their object base rather than make a separate modifier for that element.

```css
// Yep
.person {}
  .person__leg {}

.person--tall {}
  .person__leg {}

// Nope
.person {}
  .person__leg {}
  .person__leg--tall {}
```

In the HTML markup it is perfectly valid for one element to be a part of many objects. This means something like the below is beautiful.
```html
<div class="box profile profile--large">
  <img class="avatar profile__img" />
  <p class="profile__bio"> ... </p>
</div>
```

Javascript hooks should all be prefixed with `js-` and should only be styled with properties directly related to the javascript functionality. 

From SMACSS we take a few concepts like state classes, prefixed with `is-`, layout classes prefixed with `l-` and helper classes prefixed with `h-`. In this way class intent is made more obvious when reading code.
