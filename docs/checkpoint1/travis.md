### Travis CI Errors

If, when trying to run `travis login --pro --auto`, you get an error that looks like the following:

```shell script
Bad credentials. The API can't be accessed using username/password authentication. Please create a personal access token to access this endpoint: http://github.com/settings/tokens
```

Go to [http://github.com/settings/tokens](http://github.com/settings/tokens). Generate a new token, name it `travis-ci` and select **all** of the scopes underneath.

Hit Generate Token. **SAVE** this token somewhere safe (in your notes, email it to yourself, etc). You will not be able to see it again. Wherever you see `<GITHUB_TOKEN>` below, replace it with this token.

Run `travis login --pro --github-token <GITHUB_TOKEN>` in Codio. Follow the rest of step 3 in the original travis instructions (pasted below for your convenience).

---
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
(Your command should look something like this:)
```shell script
travis sshkey --generate -r cs169/hw-agile-iterations-fa20-000
```
---

You may receive another prompt for GitHub login when you run the `travis ssh` step above. If you do, enter your GitHub username as usual, and enter `<GITHUB_TOKEN>` as your password. If you see a step saying `Generating RSA key.`, you have succeeded and can move on to Travis Step 4 in the original instructions. 
