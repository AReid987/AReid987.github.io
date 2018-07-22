---
layout: post
title:      "Adding JQuery To My Rails Project"
date:       2018-07-22 17:38:23 +0000
permalink:  adding_jquery_to_my_rails_project
---


For this project I decided to add JQuery functionality to my existing project, as opposed to starting a new one from scratch. Initially, I attempted to 'ajaxify' all of the resources of the app, essentially making it a single page app. After some trial and error, i decided to instead add the ability to create and display comments for a recipe.

It was a great learning experience overall and definitely strengthened my understanding of JavaScript and AJAX. 

Here is how I met the project requirements.

1. Must render at least one index page (index resource - 'list of things') via jQuery and an Active Model Serialization JSON Backend.

For each of my Recipes, I chose to give the user the option to click a link that would load all of the comments for that recipe. This was accomplished by creating a Comment model, controller and serializer. The comment controller has an index action, which finds the recipe and it's comments, then renders the comments as JSON. In the JavaScript file, the load_comments link is hijacked via a JQuery event handler. Instead of redirecting, a get request is fired to the href of the link and the response is appended to the DOM using hooks always on the page. 

2. Must render at least one show page (show resource - 'one specific thing') via jQuery and an Active Model Serialization JSON Backend.

When a user navigates to a recipe show page, I added a next button. Clicking next of course triggers a JQuery event handler. Then a get request is fired to the route added for retrieving the next recipe. It ends up in the recipes#next_recipe action which increments the recipe id and renders it as JSON. Back in JavaScript land the JSON is passed to the showRecipe function which uses the nicely serialized @recipe (thanks to the active model serializers for my models) to append it to the DOM in place of the prior content -- all without a page refresh!

3. The rails API must reveal at least one has-many relationship in the JSON that is then rendered to the page.

When the load comments link is clicked, the has_many relationship of recipes is revealed. A recipe has_many comments. 

4. Must use your Rails API and a form to create a resource and render the response without a page refresh. 

On a recipe show page there is also a new comment form. In similar fashion to my other JavaScript/AJAX functionality, the submit event is hijacked. The difference in this case is that a post request is sent and then a JavaScript constructor is used to create a new comment object. A prototype method is then called to render this comment to the DOM. 

5. Must translate the JSON responses into Javascript Model Objects. The Model Objects must have at least one method on the prototype.

Clicking load comments link, next recipe buttons or submitting new comments will all send a request to the corresponding controller action. In any of those action the result will be render an instance of the respective class as JSON. For a new comment submission, that response will be passed into the constructor function for a new Comment object. That object will have a renderLi method. 


I first struggled with implementing some of these requirements, however I was able to benefit from that difficulty. Ultimately, it allowed for a deeper understanding of the concepts introduced by this section of the curriculum. It was satisfying to be able to render resources without a page refresh, but more so to be able to follow the flow through the program and the different languages all coming together to make it happen!
