## General Iteration Instructions

The project will be completed in 2 iterations, following the Agile methodology. Checkpoint 1 was intended for you to learn more about your project and work through all setup logistics, and Checkpoints 2 & 3 are when you will actually create code and tests.

###  Pre-iteration Planning

#### Before Starting to Code:

1. You should think of the project spec as notes from a customer (note that they are not as descriptive as previous CHIPS in terms of what you have to do functionality wise. This is to simulate the experience of working with a customer). You should make lo-fi mockups if needed, and convert all of the spec to user stories in Pivotal Tracker.

2. Your team should have a pre-iteration planning meeting to prioritize and vote on assigning points to the stories, subdividing them down as needed;  as discussed in lecture & section, each story must be ≤3 points (1 point stories are just fine).

#### Requirements prior to starting to code:

1. User stories you plan to implement this iteration are in Pivotal Tracker, with points and in priority order in the Backlog.  Label each story with “iter1”, “iter2”, etc. using Tracker's label feature. For stories that required lo-fi mockups, attach the relevant files to each Tracker story.

2. Create one or more Cucumber feature files for each user story that you plan to implement. You should write out all scenarios for that user story as we have done in class. Also write step definitions for all of the new steps that you are using, even if they are just placeholders for now. If you’re updating an existing feature, you can just edit the feature file(s) for that feature.

### Once you’ve planned out the iteration in the IPM, it's time to code!

Use the following standards to keep track of what's going on in each iteration.

* **Git & GitHub**

    1. When committing, specify the commit type (from [this list](https://github.com/commitizen/conventional-commit-types/blob/master/index.json)) at the beginning of the commit message with "[]". For example, a commit message will look like "[feat] A new feature".

        *Note: ("A new feature" is a really bad commit message. This is just to illustrate the format. Make your commit messages much more informative)*

    2. Use branch-per-feature to manage your work. When you create any branches on your GitHub repository later on as you work on the project, they will need to be linked to a specific story on Pivotal Tracker. You will need to prefix any branches you create on your GitHub repo with the story ID.

    For example, if your story ID is `123123`, any branch you create that relates to that story should be named `123123-some-branch-name`. You can read more about this in the **Using the GitHub integration: Branches** section of [this](https://www.pivotaltracker.com/help/articles/github_integration/) page.

    3. Create a Pull Request to the master branch when a feature is finished. You can use rebase to resolve conflicts on your branch. You are encouraged to squash commits into a major commit. **Remember that comments & discussion** as part of the PR form an integral part of keeping everyone on the team apprised of what's happening.

* **Pivotal Tracker** | The story states should be used as follows:

    1. `Started`: a developer or pair has claimed a story and is actively working on it, in its own branch and following the commit guidelines above.

    2. `Finished`: all tests and code for the story are complete, and a Pull Request has been opened to merge the story. Depending on the PR discussion, the story may move back from Finished to Started; if only minor changes are needed, the story will just stay in Finished until a PR for it is finally merged.

    3. `Delivered`: A PR is merged and the story's code is deployed to Heroku for the customer to try.

    4. `Accepted`: the customer has indicated (by email, or during a f2f meeting) that the story meets their original needs.

* **Daily Standups**

    As part of the agile development process, you should be completing daily standups. Especially as the whole world is remote at the moment, interfacing with your teammates regularly is critical for a functioning team.

    Our recommended workflow: every day (or as close to every day as possible), meet with your teammates as a group in a simple Zoom or Google Hangouts call. Have one person be the driver to fill out [this standup Google form](https://forms.gle/iYuk9csjkcJGaXhX7) to keep a record of your meeting. This form should be submitted by ONE team member each time you have a standup.


### End of Iteration Deliverables

1. `Retrospective survey` (1 per team): Comparing your planned iteration work with the stories actually Finished or Delivered, what could your team have done better?

2. `Peer evaluation survey` (each developer submits): We will ask you to evaluate the overall contributions of each team member during the iteration—exceeds expectations, meets expectations, somewhat/barely meet expectations, or fall short of expectations. (Consider a variety of factors: did this person communicate with rest of team effectively?  Did they try to do their share of the work? Were they prepared to work on the project, that is, did they seem to have command of the material covered in the homework?) These surveys are confidential to instructors only. We use this information to help with project grading, so please be honest and fair!

3. `Code and tests`

    * Your GSI will expect to be able to interact with the features for Delivered stories and inspect code, tests, etc. for Finished stories. The goal is to be able to Deliver all the stories you planned, but the goal is not to punish your team for failing to do so but rather to help your team understand what could have gone better so that next iteration will go more smoothly.
    * Your code coverage is 85% or higher (Coverage badge should be part of your repo's README)
    * Grade will be scaled down accordingly

Your GSIs may apply a subjective component based on the quality of your stories, interaction within team, and so on.


### Tools To Help You Get Started

Here are some tools to help you with your Iteration Planning Meetings and Retrospectives.

#### Planning Poker (for Iteration Planning Meetings):
* [Usage guide](https://drive.google.com/drive/folders/18FCUo2wOI0hKJuPh7BuGyenZuFSPtK8A)
* [Tutorial Video](https://drive.google.com/drive/folders/18FCUo2wOI0hKJuPh7BuGyenZuFSPtK8A)

**IF PLANNING POKER IS DOWN**: (e.g. if you are unable to log in or if stories aren't showing up): Watch the tutorial video above to get an idea of how planning poker is used. The same style of meeting can be conducted informally over a simple Zoom or Google Hangouts call, using the chat functionality for voting. Do not wait for Planning Poker to be fixed to conduct your IPM.

#### Postfacto (for Retrospective Meetings):
* [Usage guide](https://drive.google.com/drive/folders/18FCUo2wOI0hKJuPh7BuGyenZuFSPtK8A)
* [Tutorial Video](https://drive.google.com/drive/folders/18FCUo2wOI0hKJuPh7BuGyenZuFSPtK8A)

Next, read the [spec for Checkpoint 2](./checkpoint-2.md). 
