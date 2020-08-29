# Walkthrough: Building an Awesome Todo List App in React

Tutorial link: https://www.kirupa.com/react/simple_todo_app_react.htm

Avi: As a front-end software developer of 4 years experience, I want to assess the accessibility of this tutorial for junior devs and would-be software engineers.

To-do list apps are a popular "project to cut your teeth on" within the tech (and programing language) learning communities.

Kirupa starts of the tutorial with acknowledgement that making a to-do list app requires some skills: `we are going to tie together a lot of the concepts and techniques you've learned`

They then highlight some of the user cases/stories:

- [ ] Users can, to submit items as entries, type in an item into the input field
  - [ ] & press "Add"
  - [ ] or hit Enter/Return
- [ ] Users can see submitted items as entries
- [ ] Users can remove an item by clicking on an existing entry

## Steps

### Getting Started

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

### Creating the Initial UI

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

   Previewing in your browser should now show you the entry input field and add button.