---
title: "Add Existing Git to Github"
date: 2023-02-04T20:34:39+01:00
draft: false
---

# Adding an existing Git repository to GitHub

Let’s say you’ve just created a GitHub repository like I did and you want to push files which already you have already setup in `~/project_folder`. That could look like something like this

```
mkdir ~/project_folder
cd ~/project_folder
echo "My Awesome Project" >> README.md
```

 You init a repository like this and create you first commit.

```
$ cd ~/project_folder
$ git init
$ git add *
$ git commit -m "Init repository"
```

Now the local repository is setup and all what’s left is to connect it to the remote. On GitHub you see the following snippet:

```
$ git remote add origin <repository_url>
$ git branch -M main
$ git push -u origin main
```

When you run it, the last command will ask you for username and password.

```
$ git push -u origin master            
Username for 'https://github.com': <your username>
Password for 'https://<your username>@github.com': <your password>
remote: Support for password authentication was removed on August 13, 2021.
remote: Please see https://docs.github.com/en/get-started/getting-started-with-git/about-remote-repositories#cloning-with-https-urls for information on currently recommended modes of authentication.
fatal: Authentication failed for '<repository_url>'
```

The catch is that, instead of your actual password, GitHub (and similar services) now require you to generate a personal token. It is described in [the documentation](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/creating-a-personal-access-token), but in short: On GitHub, click on your profile image > Settings > Developer settings > Personal Access tokens > Tokens (classic).

Now fill in the name, expiration, select the scopes (`repo` should cover all you need) and after clicking Generate token, you'll see an access token. This is the thing which you'll use instead of a password.

Make sure you copy it! It says you won't be able to see it again. But no one wants you to write it on a post-it note. Before trying to `push` again, we'll make sure Git remembers the access token for the next time.

```
$ git config --global credential.helper store
```

Now let’s try if it works.

```
$ git push -u origin master                  
Username for 'https://github.com': <your username>
Password for 'https://<your username>@github.com': 
Enumerating objects: 252, done.
Counting objects: 100% (252/252), done.
Delta compression using up to 8 threads
Compressing objects: 100% (227/227), done.
Writing objects: 100% (252/252), 3.63 MiB | 586.00 KiB/s, done.
Total 252 (delta 12), reused 0 (delta 0)
remote: Resolving deltas: 100% (12/12), done.
To <repository_url>
 * [new branch]      master -> master
Branch 'master' set up to track remote branch 'master' from 'origin'.
```

I've successfully authenticated and pushed my changes! Let's see if Git remembers the credentials.

```
$ git pull
Already up to date.
```

Yes, it does. Every time I set up a remote in a different environment, I need to repeat the process and generate the access token again.
