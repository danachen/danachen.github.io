---
layout: post
title: JS for a to-do list on local storage
category: blog
---

In summary, the overall structure looks like this:

- Grab the DOM objects needed later, create an array that holds the input
- Create method that places listeners on each added item
- Create method to mark up the item inputted, call the previous method that added the listeners
- Add event listener to add button, when item is inputted, 1) array is updated, 2) storage is set, and 3) above method is executed so we have a list that's marked up properly with the correct event listneners
- Create the method to set the local storage to store the array item
- Create the get local storage method to retrieve the data, invoke the method
- Add event listnener to clear button, so when clicked on, the data structure is cleared

Similar to previous projects, setting up the JS code requires two sections, one that pulls all of the variables from the DOM for later manipulation, and the set up of methods that does the actual work.

In order to identify the variables needed, we can take a look at the HTML and the elements that holds the page together. We'll definitely need an input box, a place to output the item added (similar to pass the message), a place where error messages can be shown, the button that clears the items. Slightly less obvious, but we'll also need the form itself, which is needed to process the "submit" button that houses the "Add Item".

We also need to set up a global variable here called `list` that provides the array data structure to hold items inputted. This is beginning to seem eerily similar to the Sinatra project.

We then go through the methods one by one.

1. handleItem(): this method goes through each item to ensure it finds the buttons next to the item, including complete, edit, or delete. For each item it processes, if the text in the textContent field matches what's in the JS array, then we place 3 event listeners on the completed, edit, and delete buttons. If each one is clicked on, then the proper functionalities are called. 

Note that this method only takes care of toggling classes and adjusting the DOM display and JS array. The actual mark-up of the item and the three buttons next to it will be taken care of by the `getList()` method later on.

For "completed", that means a class toggle that shows the item completed. 

For edit, when clicked, it places the item content back in the input box and removes the content from the display section. The display array that stores all the list items is reassigned so that it filters out the array item that's just getting edited. The newly edited item will go through the input method when it's added to the list again.

When delete is clicked, the display section deletes the item, and the new array of items also filters out the item similar to the edit section.

2. getList(item): for each item in the JS array list, we append a number of HTML items to it. We need to append the text of the item, and each one of the three icons and their mark-up. We then call the handleItem() method for it. This method calls `handleItems()`.

3. setLocalStorage(): we set a local storage object from the `Window.localStorage` object, where `localStorage.setItem(name, item)` sets object in local storage.

4. getLocalStorage(): we get the item stored, and unless it does not exist, is parsed by `JSON.parse()`. Note what `JSON.stringify()`, `JSON.parse()` reverses. We then call the method. This method calls `getList()`.

5. An event listener on the submit button of the form (so this is not an API yet). Once it gets through validation, 1) push the item to the array, 2) set the local storage, and 3) getList(). We reset the input box to null.

6. An event listener on the clear button. We 1) clear the local storage, 2) clear the todoItems array, and 3) get the list that's now empty.