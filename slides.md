<!-- .slide: data-background="./reveal.js/img/title.png" -->

<!-- Presenter: -->
# Widgets 101: Customizing and Creating Widgets with the ArcGIS API for JavaScript

### JC Franco – [@arfncode](https://twitter.com/arfncode)
### Matt Driscoll – [@driskull](https://twitter.com/driskull)

---

# Welcome

![ohai](./images/ohai.gif)

---

# Agenda

- Widgets
- Building blocks
- Building our widget
  - 3.x
  - 4.x
- Tips & Tricks

---

# Widgets

- What?
  - Encapsulated
  - Cohesive
  - Single-purpose pieces of functionality 
- Why?
  - Reusable
  - Interchangeable
- How?
 - Different frameworks are available

---

# Dojo toolkit

- Foundation of ArcGIS JavaScript API
- AMD support
- Class-based inheritance
- Internationalization

![Dojo Toolkit!](images/dojo-toolkit.png)

---

<!-- JC -->
# Asynchronous Module Definition (AMD)

- Asynchronous loading
- Web-based solution
- Lazy loading
- Fewer globals
- Dependency handling

---

# AMD example

- `define`
  ```js
  // moduleA.js
  define(["moduleB"], function (moduleB) {
    // module API
    return {
      _data: 100,
      calculate: function () {
        moduleB.calculate(this._data);
      }
    };
  });
  ```
- `require`
  ```js
  // main.js
  require(["moduleA"], function (moduleA) {
    moduleA.calculate();
  });
  ```

---

# Building blocks

![blocks](./images/blocks.gif)

---

# `dijit/_WidgetBase`

what you get

- Lifecycle
  - `constructor`
  - `postMixInProperties`
  - `buildRendering`
  - `postCreate`
  - `startup`
  - `destroy`
- Events
- Getters/Setters
- Property watching

---

<!-- Matt -->
# Simple widget example

```js
/* ./MyWidget.js */
define([
  "dijit/_WidgetBase",
  // ...
], 
function(
  _WidgetBase,
  // ...
) {

  return  _WidgetBase.createSubclass({

    // widget magic here! °˖✧◝(⁰▿⁰)◜✧˖°

  });

});
```

[Simple widget](./demo/preamble/dijit-lifecycle.html)

---

# Code organization

Keep code modular and organized

---

# Code organization: HTML

- Extract HTML to separate file
- Mix in `dijit/_TemplatedMixin`
  - Renders HTML based on a template string
  - Create DOM node attachments

---

# Code organization: CSS

- Extract widget-specific styles to separate stylesheet
- @import widget stylesheet wherever applicable

---

# Example: before

```js
/* ./MyWidget.js */
define([
  "dijit/_WidgetBase",
  "dijit/_TemplatedMixin",
  "dojo/text!./templates/MyWidget.html"
],
function (
  _WidgetBase, _TemplatedMixin,
  template
) {

  return _WidgetBase.createSubclass([_TemplatedMixin], {

    templateString: "<div style='background-color: chartreuse;'>" +
                      "<label style='font-weight: bolder;'>:)</label>" +
                    "</div>";
  });

});
```

---

# Example: after

```html
<!-- ./templates/MyWidget.html -->
<div class="my-widget">
  <label class="my-widget__text">°˖✧◝(⁰▿⁰)◜✧˖°</label>
</div>
```

```css
/* ./css/MyWidget.css */
.my-widget {
  background-color: chartreuse
}
.my-widget__text {
  font-size: 1.5em;
}
```

```js
/* ./MyWidget.js */
define([
  "dijit/_WidgetBase",
  "dijit/_TemplatedMixin",
  "dojo/text!./templates/MyWidget.html"
],
function (
  _WidgetBase, _TemplatedMixin,
  template
) {
  return _WidgetBase.createSubclass([_TemplatedMixin], {
    templateString: template
  });
});
```

---

# CSS

Use classes

```html
<style>
  .my-widget {
    background-color: chartreuse;
  }
</style>

<div class="my-widget">...</div>
```

and avoid inline styles

```html
<div style="background-color: chartreuse">...</div>
```

---

# Accessibility (a11y)

- Enable your application to be used by everyone
- Use semantic markup
- ARIA roles
- Consider other input devices besides the mouse
  - Keyboard
  - Touch
  - Screen reader

---

# Internationalization (i18n)

- Keep text separate from application logic
- Support multiple languages
- Helps ease translation

```javascript
define({
  root: ({
    "button": "Home",
    "title": "Default extent"
  }),
  "ar": 1,
  ...
  "zh-cn": 1
});
```

---

# DOM manipulation

Here to help...

- `dojo/dom`
- `dojo/dom-attr`
- `dojo/dom-class`
- `dojo/dom-construct`
- `dojo/dom-style` (used sparingly)

---

# WikiWidget (requirements)

* Use Wikipedia API to geosearch for entries
* Display results in a list
* List items should center on the map and display a popup
* The popup should have a link for more info (wiki page) 

---

# Building WikiWidget

![programming!](./images/programming.gif)

---

# Building WikiWidget the 3x way

[demo](./demo/3x/index.html)

---

<!-- JC -->
# 4.x

- Widget Architecture
  - View – the face
  - ViewModel – the brain
- Reusable/Testable core widget logic sans UI concerns
- Framework compatibility
- Separates concerns

---

# Comparison

| 3.x                                     |     |     |     |     | 4.x                                     |
|:---------------------------------------:|-----|-----|-----|-----|:---------------------------------------:|
| ![3x widget way](images/widgets-3x.png) |     |     |     |     | ![4x widget way](images/widgets-4x.png) |

---

# View

- Uses ViewModel APIs to render the UI
- View-specific logic resides here 

---

# ViewModel

- Core logic of widget resides here
- Provides necessary APIs for the view to do it's thing
- No DOM/UI concerns (think data)

---

# Widget SDK

[Example: BasemapToggle](https://developers.arcgis.com/javascript/latest/api-reference/esri-widgets-BasemapToggle.html)

---

# ViewModel + frameworks

- [Angular 2](https://github.com/odoe/esrijs4-vm-angular2) – [demo](http://odoe.github.io/esrijs4-vm-angular2/)
- [React](https://github.com/odoe/esrijs4-vm-react) – [demo](http://odoe.github.io/esrijs4-vm-react/)
- [Elm](https://github.com/odoe/esrijs4-vm-elm) – [demo](http://odoe.github.io/esrijs4-vm-elm/dist/)
- [Ember](https://github.com/odoe/esrijs4-vm-ember)

---

# WikiWidget 4.x

[demo](./demo/4x/index.html)

---

# Tips & tricks

![tricks](./images/tricks.gif)

---

<!-- Matt -->
# Preprocessors

- Benefits
  - Variables
  - Mixins
  - @import & @extend
- Allow us to
  - Restyle
  - Theme
  - Write less code
- Flavors
  - [Sass](http://sass-lang.com)
  - [Stylus](http://stylus-lang.com/)
  - [Less](http://lesscss.org/)

---

# Mind specificity

```html
<style>
  #red { background-color: red; }  /* ID */

  .blue { background-color: blue; }  /* class */
  .green { background-color: green; }  /* class */

  .blue.green { background-color: yellow; }  /* 2 classes */
</style>

<div id="red"
    class="square green blue"
    style="background-color: orange"></div>
```

* [Result](http://jsbin.com/kuqunasula/edit?html,output)
* [Specificity calculator](http://specificity.keegan.st/)

---

# CSS methodologies

- Establish guidelines/rules for maintainable CSS
  - CSS & HTML best practices
  - Naming conventions
  - Ordering/grouping of CSS rules
- No silver bullet - choose what's best for your project/team
- Flavors
  - [Block-Element-Modifier (BEM)](http://getbem.com/)
  - [Scalable and Modular Architecture for CSS (SMACSS)](https://smacss.com/)
  - [Object Oriented CSS (OOCSS)](https://github.com/stubbornella/oocss/wiki)
  - [SUIT CSS](http://suitcss.github.io/)
  - [Atomic OOBEMITSCSS](http://www.sitepoint.com/atomic-oobemitscss/)

---

### Example: Block Element Modifier (BEM)

- Uses delimiters to separate block, element, modifiers
- Provides semantics (albeit verbose)
- Keeps specificity low
- Scopes styles to blocks

```css
/* block */
.example-widget {}

/* block__element */
.example-widget__input {}
.example-widget__submit {}

/* block--modifier */
.example-widget--loading {}

/* block__element--modifier */
.example-widget__submit--disabled {}
```

---

![I choose you!](images/i-choose-you.jpg)

---

## More widget sessions

- Wednesday

  - [Operations Dashboard: Extending with Custom Widgets](https://uc.schedule.esri.com/#schedule/56fb2e824be5ddcd24000aa8/56fb2e834be5ddcd24000aa9)

- Thursday

  - [Web AppBuilder for ArcGIS: Customizing and Extending](https://uc.schedule.esri.com/#schedule/56fb32e24be5ddcd24001064/56fb32e34be5ddcd24001065)
  - [Web AppBuilder for ArcGIS: Build your First Widget in 15 mins](https://uc.schedule.esri.com/#schedule/56fb2d884be5ddcd240007f1/56fb2d894be5ddcd240007f4)
  - [Extending the Operations Dashboard for ArcGIS](https://uc.schedule.esri.com/#schedule/56fb2ffe4be5ddcd24000d04/56fb2ffe4be5ddcd24000d05)

---

## Additional resources

- [This presentation (repo)](https://github.com/driskull/uc-2016-widgets-101)
- [ArcGIS API for JavaScript 4.0 SDK](https://developers.arcgis.com/javascript/)
- [Styling (4.0)](https://developers.arcgis.com/javascript/latest/guide/styling/index.html)

---

# Please take our survey

## Your feedback allows us to help maintain high standards and to help presenters

![Rate us](./images/rate-us.png)

---

# Q & A

Questions?

---

<!-- .slide: data-background="./reveal.js/img/end.png" -->