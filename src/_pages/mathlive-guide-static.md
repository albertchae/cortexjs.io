---
layout: single
date: Last Modified
title: Static Math Formulas
permalink: /mathlive/guides/static/
read_time: false
sidebar:
    - nav: "universal"
toc: false
head:
  stylesheets:
    - https://cdnjs.cloudflare.com/ajax/libs/codemirror/5.65.11/codemirror.min.css
  scripts:
    - https://cdnjs.cloudflare.com/ajax/libs/codemirror/5.65.11/codemirror.min.js
    - https://cdnjs.cloudflare.com/ajax/libs/codemirror/5.65.11/mode/javascript/javascript.min.js
    - https://cdnjs.cloudflare.com/ajax/libs/codemirror/5.65.11/mode/xml/xml.min.js
  modules:
    - /assets/js/code-playground.min.js
    - //unpkg.com/mathlive?module
  moduleMap: |
    window.moduleMap = {
    "mathlive": "//unpkg.com/mathlive?module",
    // "mathlive": "/js/mathlive.mjs",
    "html-to-image": "///assets/js/html-to-image.js",
    "compute-engine": "//unpkg.com/@cortex-js/compute-engine?module"
    };
---
# Static Math Formulas

**To render math contained in a document as a static (non-editable) formula**, 
call [`renderMathInDocument()`](/docs/mathlive/?q=renderMathInDocument) at the 
end of your document, or in a `DOMContentLoaded` event handler.

```html
<script defer type="module">
  window.addEventListener('DOMContentLoaded', () => 
    import('//unpkg.com/mathlive?module').then((mathlive) => 
      mathlive.renderMathInDocument()
    )
  );
</script>
```

By default, any LaTeX code in the text element of a DOM element that is 
enclosed with the following delimiters will be rendered as math:

- `\[`...`\]` or `$$`...`$$` -- rendered in Display Style (CSS display block)
- `\(`...`\)` -- rendered in Text Style (CSS display inline)

```html
<h1>Taxicab Number</h1>
<p>The second taxicab number is \\(1729 = 10^3 + 9^3 = 12^3 + 1^3\\)</p>
```

More complex expressions can be wrapped in a `<script>` tag. One of the 
benefits of this approach is that the browser will not attempt to display the 
content of the `<script>` tag before it is typeset, avoiding an unsightly flash
of code on screen.

**To render LaTeX code, use `<script type="math/tex">`**

**To render MathJSON, use `<script type="math/json">`**

**To render the formula inline, append** `; mode=text` **to the type**.
If no mode is provided, or `mode=display`, the display (block) style is
used.


```html
<h1>Quadratic roots</h1>
<script type="math/json"> ["Add", 
    ["Multiply", "a", ["Square", "x"]]], 
    ["Multiply", "b", "x"], 
    "c"
  ]
</script>
<script type="math/tex; mode=text">
  =  a
  \left( x - \frac{-b + \sqrt {b^2-4ac}}{2a} \right)
  \left( x - \frac{-b - \sqrt {b^2-4ac}}{2a} \right)
</script>
```

The following DOM elements are ignored for conversion: `<noscript>`,
`<style>`, `<textarea>`, `<pre>`, `<code>`, `<annotation>` and `<annotation-xml>`.

If you dynamically generate content, call [`renderMathInElement(element)`](/docs/mathlive/?q=renderMathInElement) to
render your element after the page has been loaded. This is a recursive call
that will be applied to `element` and all its children.


To render again elements or a whole document that has already been rendered,
call  `renderMathInElement()` and `renderMathInDocument()` again. This is 
useful when a change in the environment requires the layout to be updated.

To customize the behavior of the `renderMathInElement()` and `renderMathInDocument()`
functions pass an optional `options` object literal:

- `skipTags`: an array of tag names whose content will not be scanned for
  delimiters
- `processScriptType`: `<script>` tags of the indicated type will be processed
  while others will be ignored. Default: "math/tex".
- `ignoreClass`: a string used as a regular expression of class names of
  elements whose content will not be scanned for delimiters (`"tex2jax_ignore"`
  by default)
- `processClass`: a string used as a regular expression of class names of
  elements whose content **will** be scanned for delimiters, even if their tag
  name or parent class name would have prevented them from doing so.
  (`"tex2jax_process"` by default)
- `TeX.processEnvironments`: if false, math expression that start with `\begin{`
  will not automatically be rendered. (true by default)
- `TeX.delimiters.inline` and `TeX.delimiters.display` arrays of delimiters that
  will trigger a render of the content in 'textstyle' or 'displaystyle' style,
  respectively.

```javascript
renderMathInElement(document.getElementById('formulas'), {
  // Elements with a class of "instruction" or "source will be skipped
  ignoreClass: 'instruction|source',
  TeX: {
    delimiters: {
      // Allow math formulas surround by $...$ or \(...\)
      // to be rendered as textstyle content.
      inline: [
        ['$', '$'],
        ['\\(', '\\)'],
      ],
      display: [],
    },
  },
});
```
## Read-only Mathfield

When a math formula is displayed as a static element using 
`renderMathInDocument()`, the formula is transformed into some static markup.
As a result, only the markup content can be selected, not the underlying
LaTeX formula. Selection of a portion of the formula may also lead to 
unexpected results.

If preserving the ability to select a formula is important, consider
using a read-only mathfield instead.

**To create a read-only mathfield**, add the `read-only` attribute to a `<math-field>`
element.

<code-playground layout="stack">
    <style slot="style">
      .output {
        display: flex;
        align-items: baseline
      }
      math-field { font-size: 1.2em;}
      .output math-field:focus, .output math-field:focus-within {
        outline: none;
      }
    </style>
    <div slot="html">The solution of the equation is&amp;nbsp;
&lt;math-field read-only style="display:inline-block"&gt;
  x=\frac{-b\pm \sqrt{b^2-4ac}}{2a}
&lt;/math-field&gt;
</div>
</code-playground>



{% readmore "/mathlive/guides/virtual-keyboards/" %}
**Next:** Define custom layouts for the <strong>Virtual Keyboard</strong>
{% endreadmore %}




<!-- Equation rendering -->
<!-- Readonly mathfield see https://github.com/arnog/mathlive/issues/609-->
