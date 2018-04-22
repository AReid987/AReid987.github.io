---
layout: post
title:      "Project #2: Sinatra "
date:       2018-04-22 22:07:12 +0000
permalink:  project_2_sinatra
---


Going into this project, I felt very prepared. I even felt like it might be pretty easy. That was a definite underestimation on my part. Most of the time, working through the Learn curriculum, we don't begin with a blank slate. Although I felt I had a good grasp on Models, Views and Controllers, I hadn't considered that I had not yet set up a new Sinatra app from scratch. I wasn't sure where to begin. 

After some Google searching along with reviewing prior lessons and labs from the Sinatra section, I was able to figure out how to get started. I created the Gemfile, Rakefile, environment.rb and config.ru files first. When everything seemed to look correct, I created the models and assigned associations. I made a simple app for Stylists to keep a list of their clients. A Stylist has_many Clients and a Client belongs_to a Stylist. 

Next up, were the migrations. This process went smoothly enough, given all the practice provided by the curriculum. With all of this in place, it was time to drop into a Tux session to make sure I could create objects and perform all of the CRUD actions on them. I also checked that the associations were working correctly. 

I first wrote all of my controller actions in one file, with plans to handle separation of concerns later. I started with an index that provided links to sign in or sign up. My process from there was to go throught the requirements on the project page and implement routes and views to satisfy. Despite feeling that I could do more and add tons more features, I decided to heed the advice of a recent podcast I listened to about take homes for job interviews. Instead of implementing every idea I had, I would focus on satifying the project requirements and possibly add features at a later date. 

Aside from struggling a bit with the initial set up, creating routes and views to satisfy the project requirements was fairly effortless. However, I did take some extra time before getting started to review the Sinatra section and fill in areas I felt I didn't understand completely. Something I find interesting is the process of comprehension. As the many new concepts are introduced, I naturally struggle to fully grasp a lot of them. I don't notice it happening, but at some point with each topic I struggle to understand, it eventually becomes almost second nature. At the beginning of learning about MVC architecture, I really didn't get what was going on with routes and views, and especially not erb. By the time I was working on this project, it all just made sense. 

The user flow I went with is pretty simple. A Stylist can sign up or log in from the index. If logged in you are redirected from the sign up and log in links to an index of all the clients a stylist has. On the client index the options are for creating a new client or viewing an existing client which renders as a link for each client. Clicking a link takes you to a client show page, which provides options to edit or delete. Clicking the create new client button goes to the form for creating new client. All forms must be filled out and there are validations to make sure a client belongs to a stylist in order to edit or delete. There is also a log out button on the client index page. That is basically it. You must be logged in to view clients and you can only edit or delete clients that belong to you. I enjoyed working on this project and felt that it helped cement these concepts for me. 

