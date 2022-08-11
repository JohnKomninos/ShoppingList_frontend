# ShoppingList_frontend

links:
https://grocerylists-frontend.herokuapp.com/
https://github.com/JohnKomninos/ShoppingList_frontend
https://github.com/JohnKomninos/ShoppingList_Backend

purpose:
  This app lets users create a master list of items that they may need to purchase weekly. The user can then cycle through those items and create a "sublist" of items that need to be purchased on any given day. The master list always stays intact while the sublist can be deleted and recreated every time the user needs to shop.

Technology used:
  Master list page:
    1. Hide menu - this button uses "v-if functionality" to display or hide the menu options
    2. Filter buttons - depending on which button is chosen, a function is called to display only the selected items.
    3. search feature - similar to #2, the search input takes a user input and runs that through a method to then only display results that include the inputted text
    4. create shopping list - this element captures the current list of items on the page and cycles through them using a reference set to the index of the item, prompting the user to choose whether they need the item or not. If the user answers "no", the list cycles to the next item. if the user answers "yes", they are prompted to choose a quantity, and then the item is added to a model containing their sub list. Once all the items have been cycled through, the user is automatically taken to their shopping list page.
    5. post feature - the user can add new items to the master list
    6. table - the table contains all the data from the master list and allows the user to edit, delete entries as well as quick add any item to the shopping list. all functionality stays intact even after the user uses the search or filter features.

    Shopping page
    1. This page displays all items that have been added to the shopping list. As a user shops, they can delete the items off the shopping list once they have the item.

    2.The user can also edit their quantity on this page
