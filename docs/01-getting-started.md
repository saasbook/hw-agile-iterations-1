# Overview and Learning Goals

This assignment is designed to take you through the entire Agile
lifecycle of adding enhanced functionality to an existing (legacy) app.
It pulls together ideas and techniques from multiple parts of the 
[ESaaS](http://www.saasbook.info) materials, including:

* approaching and understanding a legacy codebase to identify
changepoints (Chapter 9)
* developing stories using BDD, prioritizing and assigning points,
tracking story delivery (Chapter 7)
* using branch-per-feature and pull-request-based code reviews
(Chapter 10)
* working with client-side JavaScript code (Chapter 6) as well as
Rails (Chapters 4, 5)
* creating tests for your code (Chapter 8)

The setup for the ActionMap app is more complex than the simple apps we've been
working with so far, and is more representative of a real production
app in several ways:

1. ActionMap uses quite a bit of JavaScript, so we will use Webpack to
manage the JavaScript assets, just as Bundler manages Ruby on Rails
assets.  Webpack is a sophisticated JavaScript library manager that
relies on Node.js, `npm` (the Node package manager), and other tools
that we will install.

2. The app makes use of the Google Civic Info API, for which you will
have to sign up for a (free) developer API key.  Since these keys are
sensitive you will have to securely manage how to hold on to this
data, how to get the keys into the app's production (deployment)
environment, and how to keep them in sync between production and each
team member's development environment.

In terms of the development process:

1. You will use Travis CI to run continuous integration as you
develop, and CodeCov (test coverage as a service) to measure your test
coverage each time you push changes.

2. You will use various linters--programs that check your code for
code smells, bad habits, and so on--that will run constantly each time
you push changes.

3. You will not only use Pivotal Tracker to track user stories
(features) for the app, but also enable an integration with GitHub in
order to use branch-per-feature development methodology. Specifically,
when this integration is activated, a GitHub branch can
semi-automatically remain "in sync" with the Pivotal Tracker story
that the branch implements.

    




## How to set up your local development environment
### 1. Install Ruby
You need to install Ruby. We recommend you use [rvm](https://rvm.io/) but you could also use [rbenv](https://github.com/rbenv/rbenv) to manage Ruby version.
These are environment management tools that help you switch between Ruby version easily across different projects.
Once you install one of these tools, you need to install the Ruby version listed in [Gemfile](../Gemfile).

After installing rvm or rbenv, you need to check that it is loaded in you current terminal.
You can do so by checking the version of your specific ruby env manager. 
For example if you opt for rvm:
```shell script
rvm -v
```

Note, if rvm is not loaded you might want to reload your shell profile using:
```shell script
source ~/.bashrc
```

For example, if you are using rvm run the following commands.
```shell script
rvm install 2.6.5
rvm use 2.6.5
```
Remember to replace 2.6.5 with the Ruby version in the Gemfile if it does not match.

### 2. Install Node
Similarly, you need to install Node and we would recommend that you use [nvm](https://github.com/nvm-sh/nvm) which allows you to manage multiple active Node.js versions.
The specific version of node you need to install is listed in [package.json](../package.json) under the [JSON path](https://github.com/json-path/JsonPath) `$.engines.node`.

Example use of nvm to install node version `12.13.1`:
```shell script
nvm install 12.13.1
```

### 3. Install Yarn
Once Node is installed, you need to install yarn. Node has two popular JavaScript package managers: [npm](https://github.com/npm/cli) and [yarn](https://github.com/yarnpkg/yarn).
In this project, we will use yarn as it is the default for [Rails Webpacker](https://github.com/rails/webpacker).
The specific version of yarn you need to install is listed in [package.json](../package.json) under the JSON path `$.engines.yarn`.

To install yarn run the following command, replacing `@1.22.4` with the version listed in package.json.
```shell script
npm install -g yarn@1.22.4
```

Next, run the following command to ensure you have the correct version of yarn installed.
```shell script
yarn -v
```

### 4. Install project dependencies
There are two sets of dependencies that you need to install: Ruby dependencies and Node dependencies.
First, the Ruby dependencies. [RubyGems](https://rubygems.org/) is a dependency management system that allows developers to share and distribute code, and we will use Bundler to download those dependencies.

If you haven't done so already, be sure install Bundler!
```shell script
gem install bundler
```

We recommend you install your Ruby dependencies within the project's `vendor/bundle` folder instead of installing gems globally within your Ruby installation.
To do this, run the following command:
```shell script
bundle config set path vendor/bundle 
```

The command above would imply that you will be required to run all Ruby commands with the `bundle exec` prefix.
For example, to run a migration (which we will do shortly), you must execute the command `bundle exec rails db:migrate` instead of `rails db:migrate`.
Some may find this tedious, hence we suggest an option to alias the `bundle exec` prefix. 
You could run `alias be="bundle exec"` on your terminal which will allow you to run `be rails db:migrate` instead of the more verbose `bundle exec rails db:migrate`.
You could add the alias to your shell profile eg. `~/.bashrc` to make it globally available.

For local development, we recommend you skip installing `production` dependencies by running the following command:
```shell script
bundle config set without production
```

Next, run the following command to install dependencies listed in the [Gemfile](../Gemfile).
A Gemfile is a file that list all of a Ruby project's dependencies. 
The [Gemfile.lock](../Gemfile.lock) pins the dependencies listed in the Gemfile to specific versions alongside their specific transitive dependencies.
Pinning dependencies helps different developers working on the same project have reproducible builds.

The following command will install all the requirements in the `Gemfile.lock`.
It may take a while since some of the dependencies are Ruby extensions written in C, eg. Nokogiri.
```shell script
bundle install
```

After all the gems are installed, we now need to install the JavaScript dependencies.
Node projects have a [package.json](../package.json) file that serves the same purpose as the Gemfile, but for JavaScript: listing all of the project's dependencies.
[yarn.lock](../yarn.lock) and `package-lock.json` serve the same role as `Gemfile.lock`, they pin dependencies. 
Yarn uses `yarn.lock` whereas npm uses `package-lock.json`. This project uses `yarn.lock` since it uses yarn.
Projects that use npm have `package-lock.json` instead. It is highly advised that you only have one of these and not both
in your project since they may diverge and lead to inconsistent environments for different developers. 
You will notice that `package-lock.json` is git-ignored in [.gitignore](../.gitignore).

To install JavaScript requirements, run the following command:
```shell script
yarn install
```

### 5. Run migrations and seed the database
Our app is almost ready for launch. You need to run database migrations in [db/migrate](../db/migrate) to prepare your localhost
database to store and serve data. To run migrations, execute the following command:
```shell script
bundle exec rails db:migrate
```
Note, you could use your alias `be rails db:migrate` if you opted to set it up.

Next, we need to seed our database with some data such as the list of states in the United States.
To do so, execute the following command:
```shell script
bundle exec rails db:seed
```

### 6. Launch the app!
Finally, it is time to launch the rails application and begin hacking away.
There are two ways to do this. The first option is that you could launch the application using the following bundle command.
```shell script
bundle exec rails s
```

However, this option has one major downside. It takes a long time for any changes that you make to the JavaScript to reflect on the browser since you will not have live reload.

The second option, is to have two terminals. In one terminal, run the following command:
```shell script
bin/webpack-dev-server
``` 

This will launch a `webpacker` instance that live reloads your browser as you edit the JavaScript code or CSS styles and makes the development process much faster.

After launching `webpacker-dev-server` on one terminal, switch to the second terminal window and launch a Rails server.
```shell script
bundle exec rails s
```

If you find it cumbersome to need to keep two terminal windows open, take a look at [Overmind](https://github.com/DarthSim/overmind) and how to make a [Procfile](https://devcenter.heroku.com/articles/procfile) which will run both the `webpacker` instance and Rails server at the same time.

An example Procfile might look like
```
web: bundle exec rails s
webpacker: bin/webpacker-dev-server
```

Next, you should read about [linters](./linters.md).
