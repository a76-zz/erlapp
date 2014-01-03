Erlang web application prototype.

* Steps to reproduce:
(based on the http://roberto-aloi.com/blog/2013/07/13/create-deploy-erlang-cowboy-application-heroku/#rebar)

Fetch rebar from Github and bootstrap it:
$ git clone  https://github.com/rebar/rebar.git
$ cd rebar
$ ./bootstrap
$ cd..

Initialize a new git repository and use rebar to create the skeleton for a new Erlang app.

$ git init erlapp
$ cd erlapp
$ cp ../rebar/rebar .
$ ./rebar create-app appid=erlapp

Add rebar and src dircory to git control
$ git add rebar src

Add this Readme.md to git control
$ git add Readme.md

Commit what you have done in git:
$ git commit -m "Add rebar skeleton"

* Create remote repository:
Go to http://github, create account.
Create new repository.

Push an current repository from command line:
$ git remote add origin https://github.com/a76/erlapp.git

