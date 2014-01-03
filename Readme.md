Erlang web application prototype.

Steps to reproduce:
(based on the http://roberto-aloi.com/blog/2013/07/13/create-deploy-erlang-cowboy-application-heroku/#rebar)

* Fetch rebar from Github and bootstrap it:
#+BEGIN_EXAMPLE
	$ git clone  https://github.com/rebar/rebar.git
	$ cd rebar
	$ ./bootstrap
	$ cd..
#+END_EXAMPLE

* Initialize a new git repository and use rebar to create the skeleton for a new Erlang app.

#+BEGIN_EXAMPLE
	$ git init erlapp
	$ cd erlapp
	$ cp ../rebar/rebar .
	$ ./rebar create-app appid=erlapp
#+END_EXAMPLE

* Add rebar and src dircory to git control.

#+BEGIN_EXAMPLE
	$ git add rebar src
#+END_EXAMPLE

* Add this Readme.md to git control.

#+BEGIN_EXAMPLE
	$ git add Readme.md
#+END_EXAMPLE

* Commit what you have done in git

#+BEGIN_EXAMPLE
	$ git commit -m "Add rebar skeleton"
#+END_EXAMPLE

* Create remote repository:
	Go to http://github, create account.
	Create new repository.

* Push an current repository from command line:

#+BEGIN_EXAMPLE
	$ git remote add origin https://github.com/a76/erlapp.git
	$ git push -u origin master
#+END_EXAMPLE

