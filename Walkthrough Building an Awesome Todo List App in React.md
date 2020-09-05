# Walk-through: Building an Awesome Todo List App in React

Tutorial by Kirupa, Walkthrough by Avi

### 0. Outline

1. Intro
2. User Stories
3. Dev Roadmap
4. Project Setup
5. User Interface Setup
6. Implement: Adding Items
7. Implement: Displaying the items
8. Styling our app
9. Implement: Removing Items
10. Bonus: Animation!
11. Conclusion & Thanks

### 1. Intro

Tutorial link: https://www.kirupa.com/react/simple_todo_app_react.htm

Avi: As a front-end software developer of 4 years experience, I want to assess the accessibility of this tutorial for junior devs and would-be software engineers.

To-do list apps are a popular "project to cut your teeth on" within the tech learning communities.

Kirupa starts of the tutorial with acknowledgement that making a to-do list app requires some skills: 

> "[W]e are going to tie together a lot of the concepts and techniques you've learned [so far.]" ~ Kirupa

#### Relevant fundamental React skills/techniques

- [ ] Dev can render a component
- [ ] Dev can render one or more components within another "container" component
- [ ] Dev can render a list of dynamically generated components
- [ ] Dev can pass a prop to a component
- [ ] Dev can store state within a component
- [ ] Dev can define custom functions within a component
  - [ ] Dev can add event handlers to interact-able elements
  - [ ] Dev can override default element behaviors
- [ ] Dev can add styles to component elements

### 2. User Stories

Kirupa highlights the user cases/stories:

- [ ] Users can type in an item entry into the input field
  - [ ] Users can submit typed entries by pressing the "Add" button
  - [ ] User can submit typed entries by hitting the Enter/Return key
  - [ ] Users can see submitted items as entries
  - [ ] Users can remove an item by clicking on an existing entry
- [ ] Bonus: User cannot hit or enter or click the submit button to "re-enter" the same item w/o first retyping the text

### 3. Dev Roadmap

This is the general plan of features to add, on top of setting up the project and creating the initial user interface.

1. Implement (enable the functionality of) adding items
2. Implement displaying items
3. Add styling as desired to level up visual aesthetics
4. Implement removing items
5. Add animations which play when items are added / removed

### 4. Project Setup: "Getting Started" 

1. Run (to run, type the text & hit enter) in a (preferably blank) directory: `create-react-app todolist`

2. Delete everything in the `public` and `src` folders

3. Create a new HTML document inside of the `public` folder called `index.html` with the following content:

   ```html
   <!doctype html>
   <html lang="en">
     <head>
       <title>Todo List</title>
     </head>
     <body>
       <div id="container">
     
       </div>
     </body>
   </html>
   ```

4. Create a file called `index.css` inside of the `src` folder with the following content:

   ```css
   body {
     padding: 50px;
     background-color: #66CCFF;
     font-family: sans-serif;
   }
   #container {
     display: flex;
     justify-content: center;
   }
   ```

5. Create a file called `index.js` inside of the `src` folder with the following content:

   ```javascript
   import React from "react";
   import ReactDOM from "react-dom";
   import "./index.css";
     
   var destination = document.querySelector("#container");
     
   ReactDOM.render(
       <div>
           <p>Hello!</p>
       </div>,
       destination
   );
   ```

### 5. User Interface Setup: "Creating the Initial UI"

6. To make the input field and button appear, we will create a component called TodoList. We will make a file called `Todolist.js` inside of the `src` folder with the following content:

   ```javascript
   import React, { Component } from "react";
    
   class TodoList extends Component {
     render() {
       return (
         <div className="todoListMain">
           <div className="header">
             <form>
               <input placeholder="enter task">
               </input>
               <button type="submit">add</button>
             </form>
           </div>
         </div>
       );
     }
   }
    
   export default TodoList;
   ```

7. We will make 2 changes to `index.js` inside of the `src` folder:

   ```javascript
   import React from "react";
   import ReactDOM from "react-dom";
   import "./index.css";
   import TodoList from "./TodoList"; // UPDATED
     
   var destination = document.querySelector("#container")
     
   ReactDOM.render(
       <div>
           <TodoList/> {/* UPDATED */}
       </div>,
       destination
   );
   ```

   Previewing in your browser should now show you the entry input field and add button. None of the features work just yet.

### 6. "Adding items"

8. Firstly, we'll set up the event handlers and default form handling behavior to enable the adding of new items. Add this line to `TodoList.js`:

```javascript
<div className="header">
    <form onSubmit={this.addItem}> {/*NEW*/}
        <input placeholder="enter task">
```

> "We listen for the **submit** event on the form itself, and we call the addItem method when that event is overheard. Notice that we aren't listening for any event on the button itself. The reason is that our button has a **type** attribute set to **submit**. This is one of those HTML trickeries where clicking on the button whose **type** is **submit** is the equivalent of the **submit** event on the form being fired." ~ Kirupa

9. In `TodoList.js`, in order to create the `addItem` event handler that gets called on form submit, we add the following lines of code inside of the `TodoList` class as follows (the new code goes above the `render` function) :

   ```javascript
   class TodoList extends Component {
     // NEW CONTENT STARTS HERE
     constructor(props) {
       super(props);
    
       this.addItem = this.addItem.bind(this);
     }
     
     // function stub
     addItem(e) {
    
     }
     // NEW CONTENT ENDS HERE
      .
      .
      .
   }
   ```

10. Add the following 3 lines of code inside the `constructor` function of the `TodoList` class:

    ```javascript
    super(props); // OLD CODE
    this.state = {
        items: []
    };
    this.addItem = this.addItem.bind(this); // OLD CODE
    ```

    This will allow our app to save to-do items in the app's memory (called the "state").

11. Next, we will want to store inputted entries when the form is submitted. We will use a technique called "`refs`" (which is generally not a good practice, but this is an exception to the rule, see footnote 1), by modifying the following line inside of `TodoList` component's `render` function:

    ```javascript
    <form onSubmit={this.addItem}>
    	<input ref={(a) => this._inputElement = a} {/*NEW*/}
    		placeholder="enter task">
    	</input>
    ```

    Avi's Note: While using `refs` is considered poor/bad practice, sometimes it a useful workaround. Please use `refs` with caution in the future (i.e. learn your alternatives to using `refs`)

12. We now implement the `addItem` function as follows:

    ```javascript
    addItem(e) {
      if (this._inputElement.value !== "") {
        var newItem = {
          text: this._inputElement.value,
          key: Date.now()
        };
     
        this.setState((prevState) => {
          return { 
            items: prevState.items.concat(newItem) 
          };
        });
       
        // clear the input element for reuse
        this._inputElement.value = "";
      }
       
      console.log(this.state.items);
       
      // override event's default behavior*
      e.preventDefault();
    }
    ```

    > "By default, when you submit a form, the page reloads and clears everything out." ~ Kirupa

Upon returning to the browser, upon open the developer console, you should be able to see that the input box correctly takes in new items, updates the TodoList state, and resets (clears) the input box. Items are however not yet showing up on the page.

### 7. "Displaying the items"

13. Add the following line of code to the `render` function of `Todolist`:

    ```javascript
    	</div>
    	<TodoItems entries={this.state.items}/> {/*NEW*/}
    </div>
    ```

14. Also add the following import statement to the top of `TodoList.js`:

    ```javascript
    import React, { Component } from "react";
    import TodoItems from "./TodoItems"; // NEW
    ```

15. Create a new file called `TodoItems.js` in the `src` folder and add the code as follows to it:

    ```javascript
    import React, { Component } from "react";
     
    class TodoItems extends Component {
      createTasks(item) {
        return <li key={item.key}>{item.text}</li>
      }
     
      render() {
        // 1. save passed in prop entries into a new variable
        var todoEntries = this.props.entries;
        // 2. map over new entries var with createTasks function to make array of JSX list elements
        var listItems = todoEntries.map(this.createTasks);
     
        return (
          <ul className="theList">
              {/* render out the JSX list items*/}
              {listItems}
          </ul>
        );
      }
    };
     
    export default TodoItems;
    ```

    You should now be able to, upon running the app (shell command: `npm start`), be able to add items and see them rendered as well below the input box and button.

### 8. "Styling our app"

16. Create a new style sheet file called `TodoList.css` in the `src` folder with the following style rules:

```css 
.todoListMain .header input {
  padding: 10px;
  font-size: 16px;
  border: 2px solid #FFF;
  width: 165px;
}
.todoListMain .header button {
  padding: 10px;
  font-size: 16px;
  margin: 10px;
  margin-right: 0px;
  background-color: #0066FF;
  color: #FFF;
  border: 2px solid #0066FF;
}
.todoListMain .header button:hover {
  background-color: #003399;
  border: 2px solid #003399;
  cursor: pointer;
}
.todoListMain .theList {
  list-style: none;
  padding-left: 0;
  width: 250px;
} 
.todoListMain .theList li {
  color: #333;
  background-color: rgba(255,255,255,.5);
  padding: 15px;
  margin-bottom: 15px;
  border-radius: 5px;
  list-style: none;
}
ul.theList {
  padding: 0;
}
```

17. Next, we will reference `TodoList.css` by adding the following `import` statement just below the other `import` statements inside of `TodoList.js`:

```javascript
import "./TodoList.css";
```

You can now see that the app has noticeably better looking aesthetics.

### 9. "Removing items"

A quick note on app architecture: Where code goes is generally less important for functionality, and more important for readability (i.e. how quickly your brain gets tired) and scalability (i.e. how quickly your program acquires technical debt as you increase its scope), which both contribute to code "ick" factor.

Right now the code is structured so that:

> - "The items we click on are defined in **TodoItems.js**."
> - "The actual logic for populating the items lives in our state object in **TodoList.js**."

18. Change the `return` statement under `createTasks` to look as follows:

```javascript
// "[What] we are doing is listening to the click event and associating it with an event handler called `delete`"
createTasks(item) {
  return <li onClick={() => this.delete(item.key)}
    key={item.key}>{item.text}</li>
}
```

19. Next, we will add a constructor to the `TodoItems` component and define our `delete` event handler inside the `TodoItems` component as well:

```javascript
class TodoItems extends Component { // OLD LINE
  constructor(props) {
    super(props);
 
    // "To ensure `this` resolves properly, we explicitly bind `this` in the constructor so that `createTasks` can properly resolve the `delete` function." ~ Kirupa
    this.createTasks = this.createTasks.bind(this);
  }
 
  // Note: This function does not directly delete anything itself - "It... calls *another* delete function that has been passed in to this component via props." ~ Kirupa
  delete(key) {
    this.props.delete(key);
  }
    .
    .
    .
} // OLD LINE
```

20. Next, in `TodoList.js` we will modify one line of the `TodoList` component's `render` function to appear as follows:

```javascript
<TodoItems entries={this.state.items} delete={this.deleteItem}/>
```

This adds a new prop called `delete` to the `TodoItems` component and sets it to the value of the function `deleteItem`.

21. We now define the `deleteItem` function inside of the `TodoList` component, ideally (Kirupa's recommendation) below the `addItem` function:

```javascript
deleteItem(key) {
  // "We are passing the key from our clicked item all the way here, and we check this key against all of the items we are storing currently via the `filter` method:"
  // "We create a new array called `filteredItems` that contains everything except the item we are removing."
  var filteredItems = this.state.items.filter(function (item) {
    return (item.key !== key);
  });
 
  // "This filtered array is then set as our new `items` property on our state object:"
  this.setState({
    items: filteredItems
  });
}
```

22. We are still missing a reference binding, so we add the following line to the bottom of the constructor function:

```javascript
this.addItem = this.addItem.bind(this); // OLD LINE
this.deleteItem = this.deleteItem.bind(this); // NEW LINE
```

> "This will ensure that all references to `this` inside `deleteItem` will reference the correct thing." ~ Kirupa

23. Next, we will add a mouse cursor over hover effect by editing `TodoList.css` as follows:

```css
.todoListMain .theList li {
  color: #333;
  background-color: rgba(255,255,255,.5);
  padding: 15px;
  margin-bottom: 15px;
  border-radius: 5px;
 
  transition: background-color .2s ease-out;
}
 
.todoListMain .theList li:hover {
  background-color: pink;
  cursor: pointer;
}
```

### 10. Adding Animation

24. Run from the command line, in the root folder of the to-do list app project, the following command: `npm i -S react-flip-move` (Remember - To run a command, you simply type in the command text, and then hit enter/return)

    Avi's Note: While there are options to add animations which do not require installing an external library, this particular animation library is especially convenient for this app and this tutorial. Also, it's a good opportunity to practice using the command line to install external libraries.

25. Next, add the following `import` statement to the the top of `TodoItems.js`: `import FlipMove from "react-flip-move"`

26. Now, inside of the `render` function, we will "wrap" the `listItems` JSX object with the imported `FlipMove` component like so:

```javascript
<FlipMove duration={250} easing="ease-out">
  {listItems}
</FlipMove>
```

### 11. Conclusion

Here is a working demo (does not have delete functionality though<sup>2</sup>) : https://www.kirupa.com/react/examples/todo.htm

Here is a similar demo example: https://jsfiddle.net/v4qvt8bk/

As a software developer and long time educator, I'd say that this tutorial is a great learning opportunity for junior developers and for those who are either interested to learn React or front-end software development.

Throughout the tutorial there are a few deep dives into the inner workings of React as well as the conventions of working in React, both of which contain considerable complexities. These deep dives, while few, may throw off some beginners who strive to understand everything.

I think effective learning benefits from, and quality educational materials deliver on, both an appropriate challenge level (i.e. complexity the learner can grapple with), comprehensible input, real-world context & relevance, and robust, timely feedback (see comments thread below tutorial), and a good measure of fun/engaging/interesting. This tutorial by Kirupa delivers, in my opinion, very well on all these points.

#### Avi's Rating

Complexity: ​2/3 (a few points could be simpler, perhaps footnoted)
Relevancy: 3/3 (this project would benefit any developer to learn with)
Feedback quality: 3/3 (the comments section is full of good Q&A)
Fun factor: 2/3​ (the writing is engaging & makes great use of humor)

##### Footnotes from Avi's Walk-through

Footnote 1: I strongly encourage you to read up on the Kirupa's tutorial's aside which is titled "Uncontrolled Components vs. Controlled Components". It gives further context on why `refs` can be used conjunction with `form` elements as a workaround for React app state management.

Footnote 2: This demo contains a bug where the enter key can submit the same item repeatedly - this is arguably outside the scope of this tutorial though...

