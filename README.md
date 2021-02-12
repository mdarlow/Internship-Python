# Prosper IT Consulting Internship

## Introduction

During my time at Prosper IT Consulting, I started an worked on a web application among a series of web applications using Python, Django, and PyCharm for one Sprint (two weeks). Here I built the basics of a fishkeeping app, created the model, index page, and details page. 

## Tasks

* [Build the Basic App](#build-the-basic-app)
* [Create the Model](#create-the-model)
* [Index Page](#index-page)
* [Details Page](#details-page)

### Build the Basic App

My task was to create the new app for the project, register it in settings.py, create the base and home templates in a new template folder, add function to the views in order to render the home page, link the app's home page to the overall project's main home page, and add minimal content and some basic styling to the home page. 

### Create the Model

My task was to create the models - including an objects manager for database access - for the Fish Keeping app, add a template to the app, add a views function that renders the create page, and add some basic styling to the templates.

    from django.db import models


    class FishKeepingUser(models.Model):
        first_name = models.CharField(max_length=30, blank=False)
        last_name = models.CharField(max_length=30, blank=False)
        email = models.CharField(max_length=50, blank=False)
        username = models.CharField(max_length=30, blank=False)

        Accounts = models.Manager()

        def __str__(self):
            return self.first_name + " " + self.last_name


    FISH_TYPE = [
        ('Freshwater', 'Freshwater'),
        ('Brackish', 'Brackish'),
        ('Saltwater', 'Saltwater'),
    ]


    class FishWishList(models.Model):
        fish_type = models.CharField(max_length=10, choices=FISH_TYPE, null=True)
        name = models.CharField(max_length=50, default="", blank=False)
        notes = models.TextField(max_length=300, default="", blank=True)
        budget = models.DecimalField(default=0.00, max_digits=10000, decimal_places=2)
        image = models.URLField(max_length=255, default="", blank=True)
        account = models.ForeignKey(FishKeepingUser, on_delete=models.CASCADE)

        Fish = models.Manager()

        def __str__(self):
            return self.name


### Index Page

My task was to create a new index page and link it from the home page. The user is able to create a new account, select from a list of accounts, and create an account-associated wishlist for whatever underwater pet is desired. The wishlist section of the app includes the following elements: Fish type (saltwater, brackish, or freshwater), name, notes, budget (initial price the user is willing to pay), and image. Additionally, I added in a function that gets the values of the name, type, and image elements from the database and sends them to the template, displaying them as a labeled list. The function initially displays four series of the database elements, then four more each time the user presses the "Load More" button. 

    {% extends "FishKeepingApp/FishKeeping_Base.html" %}

    {% block title %}Fish Keeping App{% endblock %}

    {% block content %}

        <h3 class="main-h">Fish Keeping App</h3>
        <hr>
            <div class="form-container">
                <form class="text-center" method="POST">
                    <label class="index-label">User:</label>
                    {% csrf_token %}
                    {{ form.account }}
                    <button type="submit">See Wish List</button>
                </form>
                <div class="text-center">
                    <h5>Add another account!</h5>
                    <button type="submit" class="new-account-button"><a href="{% url 'create' %}">Create Account</a></button>
                </div>
            </div>

    {% endblock %}


#### Functions:

      /********************/
     /* Load More button */
    /********************/
    /*
        Initially displays four
        database elements, then
        four more each time button
        is pressed.
    */
    $(function () {
        $(".js-selector-div").slice(0, 4).show();
        $("#loadMore").on('click', function (e) {
            e.preventDefault();
            $(".js-selector-div:hidden").slice(0, 4).slideDown();
            if ($(".js-selector-div:hidden").length == 0) {
                $("#load").fadeOut('slow');
            }
            $('html,body').animate({
                scrollTop: $(this).offset().top
            }, 1500);
        });
    });


      /*****************/
     /* To Top button */
    /*****************/
    /*
        Takes user back to top of
        wish list.
    */
    $('.js-selector-a[href=\\#top]').click(function () {
        $('body,html').animate({
            scrollTop: 0
        }, 600);
        return false;
    });
    $(window).scroll(function () {
        if ($(this).scrollTop() > 50) {
            $('.totop a').fadeIn();
        } else {
            $('.totop a').fadeOut();
        }
    });


### Details Page

My task was to add a details template to the template folder, create the views function that will find the instance of the database that the user selects from the index page and send it to the template, add in links for each element that will direct to their respective details page, and add basic styling to the details page template. 

    {% extends 'FishKeepingApp/FishKeeping_Base.html' %}

    {% load static %}

    {% block title %}Wish List Details{% endblock %}

    {% block styles %}

      <!--------- Compiled and Minified CSS ----------------------------------------------------------------->
      <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/3.4.1/css/bootstrap.min.css">
      <!----------------------------------------------------------------------------------------------------->
      <!--------- jQuery Library ---------------------------------------------------------------------------->
      <script src="https://ajax.googleapis.com/ajax/libs/jquery/3.5.1/jquery.min.js"></script>
      <!----------------------------------------------------------------------------------------------------->

    {% endblock %}

    {% block content %}

      <!--------------------------------------->
      <!--------------- DETAILS --------------->
      <!--------------------------------------->
      <h2 id="top" class="main-h">Wish List Details</h2>
      <div class="WishList-form-container">
          <form class="account-holder">
            <label>User:</label>
            <input type="text" value="{{ account }}" readonly>
          </form>

        <div class="container">
          <div class="row">
            <div class="col-sm-2">TYPE</div>
            <div class="col-sm-2">NAME</div>
            <div class="col-sm-2">NOTES</div>
            <div class="col-sm-2">BUDGET</div>
            <div class="col-sm-2">IMAGE</div>
          </div>
        </div>
        <br>
        <br>

        <div class="container">
          <div class="row">
            <div class="col-sm-2">{{ fish_details.fish_type }}</div>
            <div class="col-sm-2">{{ fish_details.name }}</div>
            <div class="col-sm-2">{{ fish_details.notes|linebreaks }}</div>
            <div class="col-sm-2">{{ fish_details.budget }}</div>
            <div class="col-sm-2"><a href="{{fish_details.image}}" target="_blank"><img src="{{fish_details.image}}" style="width:50px; height:50px;"></a></div>
          </div>
        </div>

        <p>Go back to <a href="{% url 'ShowWishList' fish_details.account_id %}">your wish list</a>.</p>
        <!--------------------------------------->
        <!------------- END DETAILS ------------->
        <!--------------------------------------->

        <br><br>

      </div>
    {% endblock %}

*Jump to: [Page Top](#prosper-it-consulting-internship), [Tasks](#tasks)*
