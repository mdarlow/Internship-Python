# Prosper IT Consulting Internship

## Introduction

During my time at Prosper IT Consulting, I started an worked on a web application among a series of web applications using Python, Django, and PyCharm for one Sprint (two weeks). Here I built the basics of a fishkeeping app, created the model, index page, and details page. 

## Tasks

* Build the Basic App
* Create the Model
* Index Page
* Details Page

### Build the Basic App

My task was to create the new app for the project, register it in setttings.py, create the base and home templates in a new template folder, add function to the views in order to render the home page, link the app's home page to the overall project's main home page, and add minimal content and some basic styling to the home page. 

### Create the Model

My task was to create the models - including an objects manager for database access - for the Fish Keeping app, add a template to the app, add a views function that renders the create page, and add some basic styling to the templates.

### Index Page

My task was to create a new index page and link it from the home page. The user is able to create a new account, select from a list of accounts, and create an account-associated wishlist for whatever underwater pet is desired. The wishlist section of the app includes the following elements: Fish type (saltwater, brackish, or freshwater), name, notes, budget (initial price the user is willing to pay), and image. Additionally, I added in a function that gets the values of the name, type, and image elements from the database and sends them to the template, displaying them as a labeled list. The function initially displays four series of the database elements, then four more each time the user presses the "Load More" button. 

### Details Page

My task was to add a details template to the template folder, create the views function that will find the instance of the database that the user selects from the index page and send it to the template, add in links for each element that will direct to their respective details page, and add basic styling to the details page template. 
