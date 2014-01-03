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

Commit what you have done in git:
$ git add rebar src
$ git commit -m "Add rebar skeleton"
