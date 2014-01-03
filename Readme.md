Erlang web application prototype.
=================================

**Based on:** http://roberto-aloi.com/blog/2013/07/13/create-deploy-erlang-cowboy-application-heroku/#rebar

Getting Started
---------------------------------

### Fetch rebar from Github and bootstrap it ###

	$ git clone  https://github.com/rebar/rebar.git
	$ cd rebar
	$ ./bootstrap
	$ cd..

### Initialize a new git repository and use rebar to create the skeleton for a new Erlang app ###

	$ git init erlapp
	$ cd erlapp
	$ cp ../rebar/rebar .
	$ ./rebar create-app appid=erlapp


### Add rebar and src dircory to git control ###

	$ git add rebar src

### Add this Readme.md to git control ###

	$ git add Readme.md


### Commit what you have done in git ###

	$ git commit -m "Add rebar skeleton"

### Create remote repository ###

	Go to http://github, create account.
	[I have registered as a76]
	Create new repository.
	[I have created erlapp repository]

### Push an current repository from command line ###

	$ git remote add origin https://github.com/a76/erlapp.git
	$ git push -u origin master

### Compile application ###

    $./rebar compile

Use Cowboy to create a simple web app
-------------------------------
### Create rebar.config and add the Cowboy web server as a rebar dependency ###

	$ cat rebar.config

	{deps, [
	    {cowboy, "0.8.6", {git, "https://github.com/extend/cowboy.git", {tag, "0.8.6"}}}
       	]}.

### Add cowboy to the list of applications in your .app.src file. Also, set the http_port environment variable to 8090 ###

   $ cat src/erlapp.app.src

   {application, erlapp,
 	[
	  {description, ""},
	  {vsn, "1"},
	  {registered, []},
	  {applications, [
	                  kernel,
	                  stdlib,
	                  cowboy
	                 ]},
	  {mod, { erlapp_app, []}},
	  {env, [{http_port, 8090}]}
	]}.

Modify the start/2 function from erlapp_app module so that Cowboy starts a pool of acceptors when the erlapp application is started. Configure the Cowboy dispatcher with a single dispatching rule, routing all requests to ’/’ to the erlblog_handler (see below).

	$ cat src/erlapp_app.erl

	-module(erlapp_app).

	-behaviour(application).

	%% Application callbacks
	-export([start/2, stop/1]).
	-define(C_ACCEPTORS, 100).

	%% ===================================================================
	%% Application callbacks
	%% ===================================================================

	start(_StartType, _StartArgs) ->
	    Routes = routes(),
	    Dispatch = cowboy_router:compile(Routes),
	    Port = port(),
	    TransOpts = [{port, Port}],
	    ProtoOpts = [{env, [{dispatch, Dispatch}]}],
	    {ok, _} = cowboy:start_http(http, ?C_ACCEPTORS, TransOpts, ProtoOpts),
	    erlapp_sup:start_link().

	stop(_State) ->
	    ok.


	%% ===================================================================
	%% Internal functions
	%% ===================================================================

	routes() ->
	   [
	     {'_', [
	            {"/", erlapp_handler, []}
	           ]}
	   ].

	port() ->
	   case os:getenv("PORT") of 
	   		false ->
	   			{ok, Port} = application:get_env(http_port),
	   			Port;
	   		Other ->
	   			list_to_integer(Other)
	   end.

### Let’s now implement a basic HTTP Cowboy handler, which simply replies with a 200 status code and a notorious welcoming message to any incoming request: ###

	-module(erlapp_handler).

	-export([init/3]).
	-export([handle/2]).
	-export([terminate/3]).

	init(_Transport, Req, []) ->
	    {ok, Req, undefined}.

	handle(Req, State) ->
	    {ok, Req2} = cowboy_req:reply(200, [], <<"Hello world!">>, Req),
	    {ok, Req2, State}.

	terminate(_Reason, _Req, _State) ->
	    ok.

### Finally, let’s create an interface module which will be responsible for starting your erlblog application together with all its dependencies. ###

	-module(erlapp).

	-export([start/0]).

	start() ->
	    ok = application:start(crypto),
	    ok = application:start(ranch),
	    ok = application:start(cowboy),
	    ok = application:start(erlapp).


Compile and run your application locally
-----------------------------------------

### Compile the erlapp application using rebar: ### 

	$ ./rebar get-deps compile


### Start the application and verify that everything works as expected: ###

	$ erl -pa ebin deps/*/ebin -s erlapp





















