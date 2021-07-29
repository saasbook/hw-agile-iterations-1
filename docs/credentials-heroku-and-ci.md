### Credentials Management

To manage credentials like API keys securely, we will be using Rails
5's built-in `config/credentials.yml.enc`.

Delete that existing file.  We will now generate a master key and
a new credentials file encrypted under that key:

```shell script
EDITOR=vim bundle exec rails credentials:edit
```

(You can replace `vim` with your own favorite editor.)
Save and [exit vim](https://www.google.com/search?q=how+to+save+and+exit+vim).

Later we will add keys in this file for Google Oauth, Google Civic API and GitHub Oauth.
For now, just take note of the values in the file. It should look something like this:
```yaml
.
.
secret_key_base: some-long-string
```

Rails apps can read keys from `credentials.yml.enc` by using 
`Rails.application.credentials.dig`.  This method uses the info in
`config/master.key` to temporarily decrypt the credentials file and
read the desired key(s), if they exist.  Similar to a Gemfile, the
credentials file can contain keys that are specific to particular
environments (`production`, `development`, etc.) as well as keys
common across all environments.

So, for example, given the following (unencrypted) credentials file:

```yaml
    secret_key_base: some-long-string
    production:
        GOOGLE_CLIENT_ID: prod_id
        GOOGLE_CLIENT_SECRET: prod_secret

    development:
        GOOGLE_CLIENT_ID: dev_id
        GOOGLE_CLIENT_SECRET: dev_secret
```

Then `Rails.application.credentials.dig(:production,'GOOGLE_CLIENT_ID')` would return `"prod_id"`, and
`Rails.application.credentials.dig(:development,'secret_key_base')` would return `"some-long-string"`.

You can also open a `rails console` and examine individual keys
 with `Rails.application.credentials['GOOGLE_CLIENT_ID']`
 (remember that the environment will be whatever the value of
`RAILS_ENV` was when the console was started).


### Heroku
Now we would like to setup the app on Heroku ensuring that our credentials are available there.
1. Install Heroku CLI client on your machine [using the following instructions](https://devcenter.heroku.com/articles/heroku-cli).
2. Make sure your installation works by running the following command `heroku -v` to print the version of Heroku CLI
   available on your terminal.
3. Create a new heroku app using `heroku create`. You may provide your desired appname using `heroku create fancyapp`.
   This will allow you to access your app on `fancyapp.herokuapp.com`.
   Note that if you choose to provide a name, it must be **universally unique**, or creation will fail.
4. You need to add nodejs `buildpack` to your heroku app for it to work. Do so using:
   ```shell script
   heroku buildpacks:add heroku/nodejs
   heroku buildpacks:add heroku/ruby
   ```
   To confirm that you have all the buildpacks needed, run the following command:
   ```shell script
   heroku buildpacks
   ```
   You should see both ruby and nodejs in the output.
5. Next you need to enable `PostgreSQL` on heroku for your app. Run the following command:
   ```shell script
   heroku run rails db:migrate
   ```   
   In this project, you will notice in `config/database.yml` that we are using `SQLite3` for test and development
   and `PostgreSQL` for production.
6. Run the following command to make `config/credentials.yml.enc` available on heroku:
   ```shell script
   heroku config:set RAILS_MASTER_KEY=`$(< config/master.key)`
   ```
7. Now run the typical database instructions to execute migrations and seed the database.
8. You should be able to access your application using your specific `your-heroku-1234.herokuapp.com` link.
5. Now you need to update the `credentials.yml.enc` using `EDITOR=vim bundle exec rails credentials:edit`
   for login with Google and Github to work.
   Go to [console.developers.google.com](https://console.developers.google.com), click on `Credentials` and add new `OAuth 2.0 Client IDs`.
   Copy the `GOOGLE_CLIENT_ID` and `GOOGLE_CLIENT_SECRET` and add them to your `credentials.yml.enc`. Make sure to set your callback url using your herokuapp link
   eg. `https://your-heroku-1234.herokuapp.com/auth/google_oauth2/callback`.
   Go to [github.com/settings](https://github.com/settings), click on `OAuth apps` and add a new `OAuth App`.
   Copy the `GITHUB_CLIENT_ID` and `GITHUB_CIENT_SECRET` and add them to your `credentials.yml.enc`.
   Use this [guide to generate a Google Civic Info API key](https://developers.google.com/civic-information/docs/using_api).
   Copy they Key and add it as `GOOGLE_API_KEY`
   Your final `credentials.yml.enc` should look like:
   ```shell script
    # aws:
    #   access_key_id: 123
    #   secret_access_key: 345

    # Used as the base secret for all MessageVerifiers in Rails, including the one protecting cookies.
    secret_key_base: some-key

    GITHUB_CLIENT_ID: some-key
    GITHUB_CLIENT_SECRET: some-key

    GOOGLE_CLIENT_ID: some-key
    GOOGLE_CLIENT_SECRET: some-key
    GOOGLE_API_KEY: some-key
    ```
   Commit and push your changes to heroku. Wait for 10 minutes then test that login works.
   Test out your app on your phone, see if it is [responsive](https://www.w3schools.com/whatis/whatis_responsive.asp).

### Travis CI
1. Make sure you have an account with [travis-ci.com](https://travis-ci.com).
   Create the account with the Github account that you
   use for Github Classroom.
2. Install Travis CI CLI client on your terminal using the following command:
   ```shell script
   gem install travis
   ```
3. Login into Travis CI using your Github Credentials on the terminal:
   ```shell script
   travis login --pro --auto
   ```
   Afterwards, since we need to give Travis CI a means to clone our private repo to run CI builds,
   we need to generate an ssh-key for Travis to use. First identify your repo's org and name using:
   ```shell script
   git remote -v
   ```
   If the above command prints out something similar to the following:
   ```shell script
   origin  git@github.com:[myorg]/[myrepo].git
   ```
   Replace the `myorg` and `myrepo` below and run the command below.
   ```shell script
   travis sshkey --generate -r [myorg]/[myrepo]
   ```
4. Now push your `config/master.key` to Travis CI using:
   ```shell script
   travis encrypt --pro RAILS_MASTER_KEY=`cat config/master.key` --add env
   ```
5. Then run the following command to ensure your `.travis.yml` file is valid.
   ```shell script
   travis lint
   ```

### Codecov
1. Head to [codecov.io](https://codecov.io) and Click `Student` then `Sign In with Github`.
   Identify your Github Classroom project and add the repository to Codecov.

2. You may be required to add permissions to Codecov on Github. Visit the settings page on
   Github and select `Applications`. Look for Codecov and grant extra permission if so required.

3. Visit your repo on Codecov via `codecov.io/gh/[myorg]/[myrepo]/settings`.
   Identify the `Repository Upload Token` and copy the `CODECOV_TOKEN="your-codecov-upload-token"`.

4. Add this to your repo on Travis CI dashboard. In the project's Travis page, click `More Options` then `Settings`.
   Add the token to the `Environment Variables` section.
5. Add and commit your changes locally then push to Github. Head to [travis-ci.com](https://travis-ci.com)
   to try `Trigger Build` and test the CI pipeline.

6. Now replace the Travis and CodeCov badges in README.md.
   For Travis, if you click on the `build: ....` black and green badge on your repo page on Travis, you will get option to
   copy the status image.
   For Codecov, [follow these instructions](https://stackoverflow.com/questions/54010651/codecov-io-badge-in-github-readme-md)

Next, let's set up [Pivotal Tracker](./tool_setup.md).
