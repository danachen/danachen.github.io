---
layout: post
title: Reflecting on one week of Javascript mini-projects
category: blog
---

After a week of working through some mini-projects, I have learned a couple of things about basic Javascript DOM manipulation. It's certinaly lifted the curtain behind some commonly seen JS functionalities. 
* Projects and functionalities covered:
  * Changing a background by clicking a button
  * Changing a background randomly and showing its HEX code
  * Quote generator
  * Input message in form and have it shown up elsewhere on the page
  * Counter
  * Image slider
  * Testimonials
  * Item filter from web shop
  * Item image slider

My basic takeaway from most of the JS projects and functionalities is the following:

- The big idea is to understand that you need to grab the DOM items specified with the HTML, and manipulate them with JS. In practical terms, this is a 2-step process. Within the JS script, one, assign each DOM item to a variable name, then, manipulate them through JS. 

- Any tag can become an object once retrieved in JS. Previously, I had thought the whole point of having classes and ids were only for CSS to mark up the content. But in fact, those classes and ids are also how Javascript manages to grab the value from the DOM and manipulate them as objects. 

- A few operations that I’ve come across during these projects are done by manipulating those objects’ properties like background colour, text content, etc. E.g. changing the background colour through cycling through an array of colour options, changing the background colour through a random assignment of HEX value, grabbing quotes from an array and displaying it through a “div”, adding textValue to a previously unassigned field. 

- JS can also interact directly with the users and display their input on screen, as in the example of a counter. By clicking an up or down button, the number displayed on screen will change. This is done by keeping a counter in the function, and incrementing and decrementing it depending on which is clicked. The counter is kept as a global variable and is shown as it changes. We can also apply colours to it based on a simple if … else loop, the logic is very simple.

- For the image slider and the testimonials, the tricks in getting the right look and feel is really all about setting the HTML up properly and connecting it to the right CSS elements. Awesome Font does A LOT of the heavy lifting, and has a TON of readily available symbols, stylings, etc. Along with Bootstrap, there’s very little real custom styling that needs to be done.

- For an image slider, we push the image srcs into an array, and just cycle through it. The trick to a cyclic slider is to ensure when the end is reached, we assign the index back to 0, and when 0 is reached (going backwards), we assign it to the back of the array.

- For testimonials, we venture a bit into OOP, where each customer is initialized from a Customer class. In reality, we only have the name and quote properties, but regardless, it's good exercise to “paste” each property of the object into the right DOM object and attribute.

- For the item filter project, we work with non-standard attributes, where a non-standard attribute called `filter` is accessed through the DOM. We search for the attribute either through the DOM element, or through the class names.

- Another thing I’ve noticed is that when there are multiple buttons that can be accessed, more often than not, all the buttons are saved as an array, and filtered based on their class or id properties. With many buttons, this replaces the need to assign a value to each one individually, and uses a pattern to assign behaviours.

- One thing that can get weird is how a source image is accessed. Depending on the location of the index or js file relative to the image file, might need to do `./src/img/img1.png`, or just `img/img1.png`.