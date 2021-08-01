## Checkpoint 2 Spec

### App Overview

ActionMap seeks to provide an integrated, seamless, and shareable platform that makes it easier for voters to connect with the progressive community while at the same time enabling progressive organizations, candidates, and elected leaders to reach new activists. The idea behind the application is to allow the user to visualize the political environment within all levels of government while also providing a platform to contact and voice their opinions to the decision makers within politics. This happens through attending events in their community, and sharing news articles related to a particular candidate and sharing their opinion. By asking the user to provide a news article before giving the candidate a score on a particular issue, it adds a layer of reputability to the score. Users can discover candidates by clicking on a map location, or searching for their local address.

One of the big emphasis on this project is that there will be very little hand-holding. We’ll start you off with some legacy code, which will be a basic Rails app with a few models already implemented, an external API being used (namely the Google Civic Information API), Javascript code for the map, and an asset pipeline (Webpacker).

Another important part: testing! Throughout this whole project, you’ll be expected to add tests where you see fit, leveraging both BDD with Cucumber and Capybara, as well as TDD with RSpec. CHIPS 7.7 & 8.9, as well as Quiz 4 are great resources to look at when deciding what is testable, and with which technique. When it comes to stubbing external APIs (a big portion of this project), we’ll give you some extra tips on how to do so.

For now, add `gem 'cucumber-rails-training-wheels'` *under* `gem 'cucumber-rails', require: false` on line 54 in your `Gemfile`. Then run `bundle install`.

Once installed, run `rails generate cucumber_rails_training_wheels:install`. This will generate some training wheels similar to those in CHIPS 8.9 to speed up Cucumber test writing.

#### Intended User Flow:

A user can search for candidates in one of two ways:

* **Entering their address into the search field** -OR-

* **Clicking on a state in the US Map**.
 * When a state is clicked, it will redirect the user to that state's map, on which the state's counties will be selectable. When a county is selected, it will populate the table below with that county's candidates.

Once on a particular place’s representatives list, a user can view any one representative's associated news articles, add their own, and score it.

### Part 1: Representatives

#### Representatives

The Representatives model is the first thing we’ll be working on as part of this app. It has some basic functionality; however, there’s much to be desired.

Let’s take a look at what’s already there:
We have a `representative` MVC structure already laid out. Take a look in app/views/representatives, app/models, and app/controllers.
A basic `representative` database model. This includes the representative name, OCD (Open Civic Data) ID, and office/title.
An association with `news_items`. As you can see, a `representative` has_many `news_items`.  
RepresentativesController and SearchController. The SearchController handles calling the Google Civic API.

| OCD ID | Representative Name | Representative Title | Created At | Updated At
| :------------- | :------------- | :------------- | :------------- | :------------- |
For use with Google Civic API | For use with Google Civic API | For use with Google Civic API | Auto-generated | Auto-generated

This is the preliminary database model for our App.

**TASK 1**: Refactoring Legacy Code

* There is currently an issue with the way the `civic_api_to_representative_params` method in the `Representative` model is implemented.
 * Explore the code further to understand the functionality surrounding the method, and build and understanding as to what the error may be.
 * Write a characterization test to encapsulate your understanding, and modify the code to allow the test to pass.
    (Remember Red->Green->Refactor; your test should fail before you implement a fix).

After completing this step, you may want to purge and re-initialize your database (why?), both locally and on Heroku.

Locally:
```shell script
    bundle exec rake db:drop
    bundle exec rake db:create
    bundle exec rake db:migrate
    bundle exec rake db:seed
```

On Heroku:
```shell script
    heroku run rails db:drop
    heroku run rails db:create
    heroku run rails db:migrate
    heroku run rails db:seed
```

**TASK 2**: Creating and Adding to the Representatives Profile Page

* All the information we have about our representatives are their name, OCD ID, and office! We want it to show a lot more, including address (street, city, state, zip), political party, and a photo.
    * The `representative` profile page hasn’t been created (will need to be at views/representatives/show.html.haml). You’ll need to create this.
    * The profile page should be linked from views/representatives/search.html.haml, as well as anywhere the representative name appears in views/news_items.
    * This information will come from the Google Civic Information API. See the Representatives controller and model for a basic implementation of getting some fields.
    * This will involve adding to the existing migration for the `representatives` model, so you can store the new information.
    * You’ll also need to make the representative’s Profile page look good! Don’t spend too much time styling, but you do have access to the full powers of [Bootstrap](https://getbootstrap.com/)! It should look usable when users come to your site. Be creative!
* **Testing**: Refer to ESaaS Chapter 8 Section 4 (Stubbing the Internet), and this unreleased [CHIPS assignment](https://github.com/saasbook/hw-rspec-rails-intro/blob/master/part4.md), which serves as a walkthrough for how to mock out web requests. Use these two resources to add RSpec tests that increase coverage for this portion of the app.

### Part 2: The Counties Map

For this part, you should explore the interconnectivity between the various controllers, models, and views that allow the app to search the Google Civic Information API. Starting in views/representatives/index.html.haml, trace the search code all the way to views/representatives/search.html.haml. Also, read [understand the code](./understanding-the-code.md) to see how the map works in JavaScript.

Understanding this will make the next task much easier.

**TASK 3**: Making the Map Functional

The map is broken! You can click on a state, and it’ll show you a list of counties, but there’s no way to then click on a county to show it’s representatives. It’s up to you to figure out how to fix it.
This doesn’t require a lot of code changes, but does require you to use a mix of your basic JavaScript knowledge and creativity. Figure out which URL you’ll need to modify so that a click on a county triggers the `search#search`route.

Ensure you are also writing proper tests in Cucumber for map actions.

### Part 3: Improving Coverage


**TASK 4**: Use this opportunity to increase code coverage for your app. Use CodeCov to monitor your per-file code coverage.

As noted in the iteration instructions, test coverage should exceed 85% for any files in the `/app` directory you are asked to modify or create in the tasks above. The coverage for `/app` overall should be greater than or equal to 70%.
