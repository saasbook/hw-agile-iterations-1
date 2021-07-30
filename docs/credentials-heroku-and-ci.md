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
3. Create a new Heroku app using `heroku create`. You may provide your desired appname using `heroku create fancyapp`.
   This will allow you to access your app on `fancyapp.herokuapp.com`.
   Note that if you choose to provide a name, it must be **universally unique**, or creation will fail.
4. You need to add the Ruby and Node.js `buildpack`s to your Heroku app for it to work. Do so using:
   ```shell script
   heroku buildpacks:add heroku/nodejs
   heroku buildpacks:add heroku/ruby
   ```
   To confirm that you have all the buildpacks needed, run the following command:
   ```shell script
   heroku buildpacks
   ```
   You should see both Ruby and Node.js in the output.
5. Since the version of Ruby we're using is fairly old at this point, you may need to downgrade your Heroku stack. To do so, run the following command:
  ```shell script
  heroku stack:set heroku-18
  ```
  Replace `heroku-18` with whatever stack Heroku suggests if you get an error while pushing in the next step.
6. It's time to push to Heroku! As was probably suggested when you added your buildpacks, go ahead and run:
  ```shell script
  git push heroku main
  ```
  This will push the `main` branch of your repository to Heroku and begin a build.
  If, at any point, you would like to push a different branch to Heroku just once, you can use
  ```shell script
  git push heroku <branch-name>:main
  ```
7. Next, we need to migrate our database. Run the following command to execute the typical Rails migration task on Heroku:
   ```shell script
   heroku run rails db:migrate
   ```   
   In this project, you will notice in `config/database.yml` that we are using `SQLite3` for test and development
   and `PostgreSQL` for production.
8. Run the following command to enable your Heroku deployment to read `config/credentials.yml.enc`:
   ```shell script
   heroku config:set RAILS_MASTER_KEY=$(< config/master.key)
   ```
   Double check your master key has been successfully set by running
   ```shell script
   heroku config:get RAILS_MASTER_KEY
   ```
   and comparing the result to your local [config/master.key](../config/master.key).
9. Now would also be a good time to seed your database. To encourage you to not follow this guide blindly, we won't provide the command for this step!
10. At this point, you should be able to access your application using your specific `your-heroku-1234.herokuapp.com` link by replacing `your-heroku-1234` with your Heroku app's name. If you forgot what your app was named, run `heroku apps:info` and look at the first output line.

Congrats on getting this far! Your app is now almost ready to roll! However, we need to do a couple more things to get the GitHub and Google integrations working.

1. To open and edit `credentials.yml.enc`, don't forget you'll need to run `EDITOR=vim bundle exec rails credentials:edit`. Feel free to replace Vim with your command-line editor of choice.

2. We now need to grab some credentials from Google and GitHub which will allow our app to integrate with those services. Be sure to follow all the instructions below!

   **For the Google login button**: Go to [console.developers.google.com](https://console.developers.google.com), click on `Credentials`, then under `Create Credentials` at the top, add new `OAuth Client IDs`.
   Copy the `GOOGLE_CLIENT_ID` and `GOOGLE_CLIENT_SECRET` and add them to your `credentials.yml.enc`. Make sure to add your Heroku app's link to the authorized redirect URIs
   eg. `https://your-heroku-1234.herokuapp.com/auth/google_oauth2/callback`.

   **For the GitHub login button**: Go to [github.com/settings](https://github.com/settings), click on `Developer settings`, switch to the `OAuth Apps` tab, and add a `New OAuth App`. Use your Heroku app's URL to fill out the Homepage URL and Authorization callback URL fields. (Hint: don't just copy the Google callback/redirect URL from above! Use a command to [list all the routes](https://www.google.com/search?q=rails+list+routes) in your app and find the right callback route.)
   Copy the `GITHUB_CLIENT_ID` and `GITHUB_CIENT_SECRET` and add them to your `credentials.yml.enc`.

   **For Google's Civic Info API**: Navigate to the [API Library](https://console.cloud.google.com/apis/library), search for the Civic Information API, and click "Enable." Afterwards, click the "Create Credentials" button that appears on the page you are redirected to, or use this [guide to generate a Google Civic Info API key](https://developers.google.com/civic-information/docs/using_api). Google will remind you of this after you create the key, but it is good practice to restrict the API key to only be able to access the Civic Information API in the API key settings.
   Copy the Civic Info API key and add it as `GOOGLE_API_KEY`.
   Your final `credentials.yml.enc` should look like the following (note that order and line spacing does not matter):
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
Commit and push your changes to Heroku, then test that you can login with both your Google and GitHub accounts. It may take up to 10 minutes for Google and GitHub to process your new OAuth clients, so be patient!
While you're waiting, you can see if your app is [responsive](https://www.w3schools.com/whatis/whatis_responsive.asp) on your phone.

### Travis CI
1. Make sure you have an account with [travis-ci.com](https://travis-ci.com).
   Create the account with the GitHub account that you
   use for GitHub Classroom.
2. Install Travis CI CLI client on your terminal using the following command:
   ```shell script
   gem install travis
   ```
3. Login into Travis CI using your GitHub Credentials on the terminal:
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
   Please note that as of time of writing, the above will ask you to log in again if you use 2FA on GitHub (as you probably should...), but even inputting the right credentials will still fail. (See [this GitHub issue](https://github.com/travis-ci/travis.rb/issues/413))
   Annoyingly, uploading your own SSH key via Travis CLI will fail if your SSH key does not have a passphrase (as per [another GitHub issue](https://github.com/travis-ci/travis.rb/issues/267)), so you will have to manually [generate your own SSH key](https://docs.github.com/en/github/authenticating-to-github/connecting-to-github-with-ssh) and either ensure it has a passphrase and [upload it via the CLI](https://github.com/travis-ci/travis.rb#sshkey), or [upload it manually](https://docs.travis-ci.com/user/private-dependencies/#using-an-existing-key).
4. Now push your `config/master.key` to Travis CI using:
   ```shell script
   travis encrypt --pro RAILS_MASTER_KEY=`cat config/master.key` --add env
   ```
5. Then run the following command to ensure your `.travis.yml` file is valid.
   ```shell script
   travis lint
   ```

### Codecov
1. You'll need to claim your [GitHub Student Developer Pack](https://education.github.com/pack) before beginning.
1. Head to [codecov.io](https://codecov.io) and `Sign In with GitHub`.
   Identify your GitHub Classroom project and add the repository to Codecov.

2. You may be required to add permissions to Codecov on GitHub. Visit your [GitHub settings page](https://github.com/settings) and select `Applications`. Look for Codecov and grant extra permission if needed.

3. Visit your repo on Codecov via `codecov.io/gh/[myorg]/[myrepo]/settings`.
   Identify the `Repository Upload Token` and copy the `CODECOV_TOKEN="your-codecov-upload-token"`.

4. Add this to your repo on Travis CI dashboard. In the project's Travis page, click `More Options` then `Settings`.
   Add the token to the `Environment Variables` section.
5. Add and commit your changes locally then push to GitHub. Head to [travis-ci.com](https://travis-ci.com)
   to try `Trigger Build` and test the CI pipeline.

6. Now replace the Travis and CodeCov badges in README.md.
   For Travis, if you click on the `build: ....` black and green badge on your repo page on Travis, you will get option to
   copy the status image.
   For Codecov, [follow these instructions](https://stackoverflow.com/questions/54010651/codecov-io-badge-in-github-readme-md)

Next, let's set up [Pivotal Tracker](./tool_setup.md).
