## Checkpoint 3 Spec

In the following parts, ensure you are writing tests and keeping coverage for the `app` directory `>=70%`, and for any files you modify `>=85%`.

### Part 4: News Articles + Issues + Ratings

The Representatives table is currently associated with many news articles. What we’d like you to do in this section is add a new `"Issue"` column to News Articles to make it more specific, and a new model `"Ratings"` to get a particular user’s opinion of a candidate’s view on an issue, based on a news article.

**TASK 5**: Set up the News Article Rating infrastructure

* For a news article, add an ‘Issue’ column (similar to adding a Director column to Movies in CHIPS 8.9) to News Articles to relate an article with a particular issue.
    * This field should be populated by a dropdown on the ‘Create News Article’ page, to associate an issue with a particular article. It should be drawn from the following list:
`["Free Speech", "Immigration", "Terrorism", "Social Security and Medicare", "Abortion", "Student Loans", "Gun Control", "Unemployment", "Climate Change", "Homelessness", "Racism", "Tax Reform", "Net Neutrality", "Religious Freedom", "Border Security", "Minimum Wage", "Equal Pay"]`

* You should also add a new migration for the `Ratings` model, which should be associated with News Articles in the following way: a `news_item` should `have_many` `ratings`, and a `user` should `have_many` `ratings`.
    * A single `rating` associated with a news article should be populated by a dropdown on the ‘Create News Article’ page, to associate a rating with a particular article. A rating should be on a scale of 1-5.


### Part 5: Your Own External API

**TASK 6**: This part of the project comes with a choice: whether you’d like to augment the Representatives portion of the app, or the News Articles portion. In either case, you'll be selecting an external API to use.

Secrets management will play an important role here (similar to how we set up the Google Civic Information API), whichever option you end up choosing, as will stubbing out and testing external APIs. Refer to the earlier Representatives section for a resource on stubbing out the internet.

**Option 1: ProPublica Campaign Finance API**: Your first option for this project is to add additional details about campaign finances for US Presidents, Senators, and Congressional Representatives.

[ProPublica Campaign Finance API](https://projects.propublica.org/api-docs/campaign-finance/candidates/#get-top-20-candidates-in-specific-financial-category) is the link to the API (specifically, to the “Get Top 20 Candiates in a Specific Financial Category” which we'll be using).

The deliverable:
* add a new model, controller, and set of views for the `Campaign Finance` information we'll be accessing. Similar to the `Representatives` search page, we'll want to add a new search page to the navbar called `Campaign Finances`.

* the `Campaign Finances` search page should have 2 dropdowns, one for each of the parameters (`cycle` and `category`) to the ProPublica API, as well as a `Search` button. Ensure you restrict the available options in the dropdowns to what the API allows.

* Once a user selects a specific category and election cycle, allow them to click a `Search` button to show a table with the top 20 candidates in that category that election cycle.

**Option 2: News API**: Your second option for this task is to use the News API to pre-populate a list of news articles that the user can choose from to rate for a particular representative.

[News API](https://newsapi.org/s/google-news-api) is the link to the API.

* Currently, the new news article page (`/representatives/[id]/my_news_item/new`) has fields for Title, Link, Description, Representative, Issue, and Rating. Split this into two pages, where a user can first select the Representative and Issue they desire, and click `Search`.
* This should take them to a second page, where there is a list of the top 5 articles returned from the News API, and they can select one via **radio button**. This list of articles comes from making a query to the News API, and getting the top 5 titles, URLs, and descriptions.
* A user should be able to select a radio button for their desired article, rate it, and hit the `Save` button to add it to the database.
* See below for a lo-fi mockup. Text in `blue` is user input, and in `red` are notes.

![](news_item_render.jpg)
