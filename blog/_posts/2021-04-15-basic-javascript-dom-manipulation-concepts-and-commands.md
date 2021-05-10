---
layout: post
title: Basic Javascript DOM manipulation concepts and commands
category: blog
---

## Specifications when Javascript runs in a web browser?
- DOM spec: all page contents are represented as object and can be modified
- BOM spec: additional objects provided by browser, non-DOM objects like the `navigator` or `location` objects
- HTML spec: describes HTML language (tags) and BOM, various browser functions like `setTimeout`, `alert`, `location`

## The DOM tree represent?
The document in the browser is the DOM tree. Tags become element nodes, text become text nodes. All nodes can be manipulated.

## Wys of reaching a DOM node's immediate neighbours

all nodes | element nodes only
------------------------------
parentNode | parentElement
childNodes | children
firstChild | firstElementChild
lastChild  | lastElementChild
previousSibling | previousElementSibling
nextSibling | nextElementSibling

## Properties supported by the <table> element

element | properties | references
----------------------------------
table |
      | .rows | <tr>
      | .caption | <caption>
      | .tHead | <thead>
      | .tFoot | <tfoot>
      | .tBodies | <tBody>
thead | .rows | <tr>
tfoot | .rows | <tr>
tbody | .rows | <tr>
tr | .cells | <td>, <th>
   | .sectionRowIndex | position index of <tr>
   | .rowIndex | number of <tr> in table, including all table rows
td | .cellIndex | number of cell inside <tr>
th | .cellIndex | number of cell inside <tr>

## Main methods to search for nodes in DOM

Method | Searches By | Can call on element? | Live?
----------------------------------------------------
querySelector | CSS-selector | yes | no
querySelectorAll | CSS-selector | yes | no
getElementById | id | no | no
getElementsByName | name | no | yes
getElementsByTagName | tag or '*' | yes | yes
getElementsByClassName | class | yes | yes

Additionally:
`elem.matches(css)` checks if `elem` matches a given CSS selector
`elem.closest(css)` looks for nearest ancestor that matches CSS selector, including the `elem` itself
`elemA.contains(elemB)`, returns `true` if `elemB` is inside `elemA`, or if `elemA === elemB`

## Differences between a live and a static collection
Methods like `querySelectorAll` returns a static collection, as opposed to `getElementsByTagName`.

```html
<div>First div</div>
<script>
let divs = document.getElementsByTagName('div');
alert(divs.length); // 1
</script>
<div>Second div</div>
<script>
alert(divs.length); // 2
</script>
```

```html
<div>First div</div>
<script>
let divs = document.querySelectorAll('div');
alert(divs.length); // 1
</script>
<div>Second div</div>
<script>
alert(divs.length); // 1
</script>
```

## Main DOM node properties

DOM node properties | HTMLInputElement(<input>) | HTMLAnchorElement(<a>)
-------------------------------------------------------------------------
nodeType | value | href
nodeName (all) | type |
tagName (for elements)
innerHTML
outerHTML
textContent
nodeValue (mostly uses data below)
data (content of non-element node, modifiable)
hidden (when true, equals `display: none`)

# Differences between attributes (HTML) and properties (DOM)

In general, attributes are what's written in HTML, and properties are what's in DOM objects.

     | Properties | Attributes
-----------------------------------------------------------------------------------
Type | Any value, standard properties have types described in the spec | A string
Name | Name is case-sensitive | Name is not case-sensitive

We mostly use properties, but can choose attributes only if DOM properties do not suit, or when exact attributes are needed.
- Non-standard attribute, if starts with `data-`, use `dataset`
- Read value as written in HTML, e.g. the `href` field is always a full URL and we want the full value

Methods that work with attributes:

elem | 
----------------------------
     | .hasAttribute(name)
     | .getAttribute(name)
     | .setAttribute(name, value)
     | .removeAttribute(name)
     | .attributes


E.g. get the attribute of an element with a non-standard attribute
<div data-widget-name="menu">Choose genre</div>
<script>
  let elem = document.querySelector('[data-widget-name]');
  console.log(elem.getAttribute('data-widget-name'));
  console.log(elem.dataset.widgetName); // does the same as above
</script>