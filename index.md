# Web Components - Quick overview

Web Components rock! :rocket:

1. [ Concepts ](#concepts-cake)
2. [ History ](#history-doughnut)
2. [ Libraries ](#libraries-lollipop)
2. [ Examples ](#examples-cupcake)

______________

## Concepts :cake:

### Custom Elements

- Provide a way for authors to build their own fully-featured DOM elements. [Reference](https://html.spec.whatwg.org/multipage/custom-elements.html)
- With Custom Elements, web developers can create new HTML tags, beef-up existing HTML tags, or extend the components other developers have authored. The API is the foundation of web components. It brings a web standards-based way to create reusable components using nothing more than vanilla JS/HTML/CSS. The result is less code, modular code, and more reuse in our apps. [Reference](https://developers.google.com/web/fundamentals/web-components/customelements)

Example:

> Autonomous type
```
class MyPopup extends HTMLElement {
  constructor() {
    super();
    ...
  }
}
customElements.define('my-popup', MyPopup);

// Used as: 
<my-popup></my-popup>

```

> Built-in type (Expands an existent DOM element)

```
class MyList extends HTMLUListElement {
  constructor() {
    super();
  }
}
customElements.define('my-list', MyList, { extends: "ul" });

// Used as:
<ul is="my-list">...</ul>

```

### Shadow DOM :banana:

- An important aspect of web components is encapsulation — being able to keep the markup structure, style, and behavior hidden and separate from other code on the page so that different parts do not clash, and the code can be kept nice and clean. The Shadow DOM API is a key part of this, providing a way to attach a hidden separated DOM to an element. [Reference](https://developer.mozilla.org/en-US/docs/Web/Web_Components/Using_shadow_DOM)

- "DOM...in the shadows" - Shadow DOM is just normal DOM with two differences: 1) how it's created/used and 2) how it behaves in relation to the rest of the page. Normally, you create DOM nodes and append them as children of another element. With shadow DOM, you create a scoped DOM tree that's attached to the element, but separate from its actual children. This scoped subtree is called a shadow tree. The element it's attached to is its shadow host. Anything you add in the shadows becomes local to the hosting element, including `<style>`. This is how shadow DOM achieves CSS style scoping. [Reference](https://developers.google.com/web/fundamentals/web-components/shadowdom#what)

:bulb: Note

There are some important Shadow DOM concepts:

- _*Shadow host*_: The regular DOM node that the shadow DOM is attached to.
- _*Shadow tree*_: The DOM tree inside the shadow DOM.
- _*Shadow boundary*_: the place where the shadow DOM ends, and the regular DOM begins.
- _*Shadow root*_: The root node of the shadow tree.

![ShadowDOM diagram](/assets/shadow.png)
[Reference](https://developer.mozilla.org/en-US/docs/Web/Web_Components/Using_shadow_DOM)

Example:

```
const div = document.createElement('div');
const shadowRoot = div.attachShadow({mode: 'open'});
shadowRoot.innerHTML = '<span>Hello Shadow DOM</span>'; // Could also use appendChild().

// div.shadowRoot === shadowRoot
// shadowRoot.host === div
```


### HTML Templating

- The template element is used to declare fragments of HTML that can be cloned and inserted in the document by script. [...] In a rendering, the template element represents nothing. [...] The template contents of a template element are not children of the element itself. [Reference](https://html.spec.whatwg.org/multipage/scripting.html#the-template-element)

-  Templates allow you to declare fragments of markup which are parsed as HTML, go unused at page load, but can be instantiated later on at runtime. [Refernce](https://www.html5rocks.com/en/tutorials/webcomponents/template/)

Example:
```
<html>
<head></head>
<body>
  <template><h1>hi</h1></template>

  <template id="mytemplate">
    <img src="https://www.html5rocks.com/static/images/profiles/75/ericbidelman.75.png" alt="great image">
  </template>

</body>
</html>
```

:bulb: Note. To use a template we need to _activate it_. If we don't then its content will never render.
Two examples of activating it:

```
const fragment = document.getElementById('mytemplate').content.cloneNode(true);
document.body.appendChild(fragment);
```
or by using the importNode method which copies from a document into another one.

```
const template = document.getElementById('mytemplate');
const clone = document.importNode(template.content, true);
document.body.appendChild(clone);
```

> :bulb: Check this link to review composition of components with HTML templates and slots [Link](https://developers.google.com/web/fundamentals/web-components/shadowdom#composition_slot)



______________
## History :doughnut:

First attempts:

> 1998 - Microsoft HTML Components (HTC) "HTML Component (HTC for short) provides a mechanism for reusable encapsulation of a component implemented in HTML, stylesheets and script." - [Reference](https://www.w3.org/TR/NOTE-HTMLComponents)

Example:

```
// example.htc

<public:component>
  <HTML xmlns:PUBLIC="urn:HTMLComponent">
  <PUBLIC:METHOD NAME="startFlying" />

  <SCRIPT LANGUAGE="JScript" >
  function startFlying()
  {
    // insert flying code here
  }
  </SCRIPT>
</public:component>

// Used as:

<html>
  <head>
    <style>
      h1{ behavior: url(example.htc) }
    </style>
  </head>
<body>
  <h1></h1>
</body>
</html>
```

> 2001 - XBL1.0 - "Extensible Binding Language is a XML-based markup language to implement reusable components (bindings) that can be bound to elements in other documents." - [Reference](https://developer.mozilla.org/en-US/docs/Archive/Mozilla/XBL/XBL_1.0_Reference). [Especification](https://www.w3.org/TR/2001/NOTE-xbl-20010223/)

> 2007 - XBL2.0 - "XBL is a mechanism for overriding the standard presentation and interactive behavior of particular elements by attaching those elements to appropriate definitions, called bindings." - [Reference](https://www.w3.org/TR/xbl/)

This is how it would look like if we had used XBL2.0 web components:

```
<xbl xmlns="http://www.w3.org/ns/xbl">
 <binding id="nav-then-main">
  <template>
   <div id="wrapper">
    <div id="col2"><content includes=".nav"/></div>
    <div id="col1"><content includes=".main"/></div>
   </div>
  </template>
  <resources>
   <style>
    #wrapper { display: table-row; }
    #col1, #col2 { display: table-cell; }
   </style>
  </resources>
 </binding>
</xbl>
```

Used as:

```
<html>
 <head>
  ..
 </head>
 <body>
  <div class="main"></div>
  <div class="nav"></div>
 </body>
</html>
```

The result of the above is as if one simple had written:

```
  <div class="nav"></div>
  <div class="main"></div>
```

______________
## Libraries :lollipop:


Some libraries based on the Web Components standard:

- [LitElement](https://lit-element.polymer-project.org/)
- [Polymer](https://polymer-library.polymer-project.org/)
- [Stencil](https://stenciljs.com/)
- [Hybrids](https://hybrids.js.org/)
- [Slim](https://slimjs.com/#/getting-started)

______________
## Examples :cupcake:

- https://github.com/mdn/web-components-examples

- [In construction Web APIs web components...](https://github.com/calamarzone/web-apis)