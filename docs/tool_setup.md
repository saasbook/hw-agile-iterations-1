## Tool Setup

### 1. Set up Pivotal Tracker

For this project, you'll be using Pivotal Tracker to track user stories, estimate story points, etc. First, all team members should create Pivotal Tracker accounts (which is free); then _one_ member creates a new project, and add your teammates as owners.

Your Pivotal Tracker project should be titled **Actionmap-Su21-GroupXX**, with the **XX** filled in with your group number (found on the team assignment spreadsheet).

Configure your Pivotal Tracker project settings as follows:
 * Start Iterations On: Friday
 * Project Start Date: 2021-08-04
 * Iteration Length: 2 weeks
 * Point Scale: Power of 2 (0, 1, 2, 4, 8)
 * Initial Velocity: 10
 * Velocity Strategy: Average of 1 iteration
 * Number of Done Iterations to Show: 4
 * *Uncheck Plan Current* Iteration Automatically
 * Follow [this tutorial](https://www.pivotaltracker.com/help/articles/github_integration/) to set up GitHub integration for the Pivotal Tracker project.  Note that:
   * The setting is in your Pivotal Tracker project instead of GitHub repository.
   * You should connect the project with the forked repository instead of the golden repository.
   * Only the owner of your project can create this integration.

#### Important: when you create any branches on your GitHub repository later on as you work on the project, they will need to be linked to a specific story on Pivotal Tracker. You will need to prefix any branches you create on your GitHub repo with the story ID.

For example, if your story ID is `123123`, any branch you create that relates to that story should be named `123123-some-branch-name`. You can read more about this in the **Using the GitHub integration: Branches** section of [this](https://www.pivotaltracker.com/help/articles/github_integration/) page.

### 2. Add Auditor Accounts

- Under the Members tab of your Pivotal Tracker project, add *cobyzhong* and *governifyauditor* as **Owners**.
- Under the Access tab in your Heroku project, add *coby.zhong@berkeley.edu* and *governify.auditor@gmail.com* as **Collaborators**.

Next, explore and [understand the codebase](./understanding-the-code.md).
