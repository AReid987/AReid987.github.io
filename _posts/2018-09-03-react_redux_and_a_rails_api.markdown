---
layout: post
title:      "React Redux and a Rails API"
date:       2018-09-03 15:47:47 +0000
permalink:  react_redux_and_a_rails_api
---


For my final project (yeeee!) for the Flatiron School Fullstack Web Dev Bootcamp, we were to make an app using a Rails/Redux frontend and a Rails backend. I decided to go with a domain model of a note taking app. 
The app is pretty simple at the moment, however I plan on building out more functionality and improving the design in time. 

An interesting thing I found about React is how quickly it evolves. Searching around the interwebs for articles on React, I quickly realized that if an article was more than a year or so old, chances are any code examples would need to be translated to a current implementation. I actually think that this helped cement my understanding on quite a few topics. Also, there seems to be so many different ways in which one can use React in a stack. I think that flexibility is an aspect of what makes React powerful and so widely adopted. 

In my process of creating this app, I first began with the create-react-app. I didn't do much more on the front end initially, aside from generating the configurations and file structure. Using create-react-app was reminiscent of Rails in the automagic terminal creation. The first real task I worked on was creating the backend API. I had not previosly generated a Rails app with the API flag, so that was new to me. I started with just one model for notes, with the first go being to see some JSON data in my browser after navigating to the server. A note model, routes, and controller, plus a serializer that will come in handy later when I add more models was the starting point. I had my controller return records from the database as JSON. I was able to boot up Rails and see some records I had seeded the database with. 

Next up was using foreman to connect the two apps (frontend and backend) together. I created the Procfile and used a proxy in the package.json file to make that work. Then I could avoid some cors issues. I also made use of the rack-cors gem. After this I installed some dependencies, such as react-redux, redux, react-router-dom, etc. 

With everything wire up and dependencies included, it was time to start making some components. I added some routes to my app component and then created a header which is purely presentational. The header user NavLink to navigate between components. In the app component, routes are defined so specific components render when the NavLinks are clicked. 

After making some basic components and navigation, I moved on to configuring the store. Even though I only have one reducer currently, I used combineReducers for scalability. I configured the store, reducer and a noteActions file. I made use of the Provider component from redux to wrap all of my components and pass in the store. The first action creator I implemented was for the index view. In the action, first an action of 'LOADING_NOTES' is dispatch, then I make a fetch request to the Rails API, which returns all instances of the Note class. Once the promise resolves, I then dispatch a new action which will trigger the reducer to return a new copy of the state. This is going to trigger any components connected to the store to re-render.

The reducer is essentially a function that sets a default state and accepts an action as the second argument. In the body, a switch statement determines the return value. The different action creators will return a dispatch with the action type to trigger the cases in the reducer. In writing this app, the flow of Redux finally started to make a lot more sense. A user action, such as a click, or a component lifecycle event, can trigger an action and if the state changes the connected components with re-render. 

To show all of my notes, I made a container component. This component called NotesPage is connected to the store and thus, has access to the state. It can then pass the state to child components as props. It is simply responsible for managing the state, leaving presentation to other components. I made use of mapStateToProps in order to pass the notes from state into the presentational component NotesList as props. The list of notes is rendered in the presentational component with no knowledge of state. 

I wanted the functionality that when a note from the list is clicked it displays dynamically. I made another container for the show feature, and also made use of a nested route using the id of the note to be displayed. When a note from the list is clicked, it is found by id from the current collection and rendered. 

A similar pattern was used for both editing notes and creating new notes. For editing, there is a boolean key in the state. When a note is rendered for the show feature, it has an edit button. Clicking that button changes the isEditing value in state, which triggers the NoteForm to render and be hydrated with that note's data. Changes trigger an update action which will make a PUT request to the server. The NotesController update action will make those changes to the database, finally returning the updated note as JSON. The isEditing state gets toggled back to false the the updated component will show. 

The same form is used to make a new note. The button text changes based on conditional logic passed in as props. A form submission will trigger an action, make a POST request and the NotesController create action is triggered. The new note will then be added to the list. 

This project was a very valuable learning tool for React and Redux and I am looking forward to making more apps with this stack!
