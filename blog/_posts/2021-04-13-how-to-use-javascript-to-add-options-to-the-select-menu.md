---
layout: post
title: How to use Javascript to add options to the select menu
category: blog
---

While learn the basics of HTML, we typically place all options for a `select` like this.

``` html
<label for="select-drink">Pick your favourite drink</label>
  <select class="form-control" id="input-menu">
       <option selected value="0">Choose...</option>
       <option selected value="1">Coca-Cola</option>
       <option selected value="2">Ice Tea</option>
       <option selected value="3">Sprite</option>
       <option selected value="4">Ginger Ale</option>
   </select>
```

But in real life, we'll probably want to have the `select` menu fed through Javascript. So how do we do that?

For simplicity's sake, we have no external data coming in through a database, and we'll simply retrieve the data from JS.

1. We set up an array of data that can be displayed later.

`const drinks = [{value: 1, name: "Coca-Cola"}, {value: 2, name: "Ice Tea"}, {value: 3, name: "Sprite"}, {value: 4, name: Ginger Ale}];`

2. We iterate through the array, and find a way to attach the array values to an appropriate DOM element.

``` javascript
drinks.forEach (function(drink) {
 ....
}
```

3. We need to create a DOM element to hold each one of the array values. 
`const option = document.createElement('option')`

4. For each element created, we can assign it the array object's key-value pairs.

`option.textContent = drink.name` gives each option a name, and `option.value = drink.value` would pass the order of the options to the `selected value = ` attribute field.

5. We need to get the `select` DOM to a variable: `const select = document.querySelector('#input-menu')`

6. This parent element needs to append the newly created element called `option`, one by one, to its menu of choices: `select.appendChild(option)`.

In summary, in order to provide the HTML with a more dynamically determined list of data (in this case the `select` menu), the JS can do the following: create an array to hold the data. Then iterate through the data, create a new element object to display the data, assign the appropriate data properties (in this case, `textContent` and `value` fields for a `select` menu) to the array object key-value pairs. Then, we collect the parent element (in this case `select`), and append each child element to it.

``` javascript
drinks.forEach (function(drink) {
  const option = document.createElement('option');
  option.textContent = drink.name;
  option.value = drink.value;
  const select = document.querySelector('#input-menu');
  select.appendChild(option);
}
```