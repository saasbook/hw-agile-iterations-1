A linter is a program that analyzes source code for common errors and conformance to a coding style.
When working in a team, it helps to have the repository conform to the same coding style.
For example, a specific team would like to have a specific template rendering system, say [erb](https://apidock.com/ruby/ERB) over [haml](http://haml.info/tutorial.html).
These standards tend to make a codebase consistent and hence easier to work with. 
Such decisions are made on a team-by-team basis and each team may have different preferences resulting in different styles being adopted.
Regardless of the final decision, teams tend to benefit overall from picking a standard or style and sticking by it.


You may have noticed that most of our HTML in [app/views](../app/views) is rendered using HAML. Our team decided to use it as it is more cross-platform,
that is, you can find HAML engines in more programming languages including Java, Javascript and PHP. This makes it easy for most developers to work with the codebase.


To enforce coding standards within this project, we have setup three
different linter systems that enforce this project's formatting
standards.  We have included tests that will enforce the linter checks
in [spec/linters](../spec/linters) that are run automatically on CI.


[rubocop](https://github.com/rubocop-hq/rubocop) checks your
Ruby code and relies on [.rubocop.yml](../.rubocop.yml) for configuration.
You can manually do this checks with `bundle exec rubocop`; add `-a` to autocorrect some
common errors (but as always, commit before you do so!).

[haml-lint](https://github.com/sds/haml-lint), configured by
[.haml-lint.yml](../.haml-lint.yml), works on Haml files.
`bundle exec haml-lint` runs it manually.

Finally,
[eslint](https://eslint.org/), configured by
[.eslintrc.js](../.eslintrc.js), works on JavaScript.
`yarn run lint_fix` runs it manually.  (You can see in
[package.json](../package.json) that `lint` and `lint_fix` are entries
in JSON path `$.scripts.lint` and `$.scripts.lint_fix` that invoke a
vendored install of `eslint`.) 


**Tip:**
[Guard](https://github.com/guard/guard) is a tool that allows you to run file watchers that run automated tasks whenever a file or directory is modified.
The [Guardfile](../Guardfile) specifies file watchers and tasks to
run. 
One option is to setup Guard to run the linters every time you modify
a file.
To initiate guard, run the following command in a separate terminal:
```shell script
bundle exec guard
```

Next, you should read about [Credentials management, Heroku setup and CI Setup](credentials-heroku-and-ci.md)
