Application
===========

An online gallery of vicious circles.

It's an application to let people share insights on [vicious circles][Vicious circle] they are affected by or they know about. Also it lets participants share their solutions to the circles and vote for them.

Learn more at https://github.com/lzrski/catch-22

This file is an entry point of the app (see [./package.json][]).

The app is [Flatiron][] based. Templates are powered by [Creamer][] - an excelent [Coffecup][] plugin with MVC capabilities. User authentication uses [Mozilla Persona][] with my humble [plugin][Flatiron Persona], which requires session and cookie parser middleware from [Connect][]. 

    flatiron = require 'flatiron'
    creamer  = require 'creamer'
    persona  = require 'flatiron-persona'
    connect  = require "connect"
    app      = flatiron.app

Runtime configuration is done with [nconf](https://github.com/flatiron/nconf)
    
    app.config.defaults {
      host    : "localhost"
      port    : 4000
      secret  : "Kiedy nikogo nie ma w domu, Katiusza maluje pazury na zielono i głośno się śmieje swoim kocim głosem. To prawda!"
    }
    # TODO: Doesn't read ?
    app.config.file file: __dirname + '/../config/config.json' 

    app.use flatiron.plugins.http
    # TODO: this should be somehow configurable
    app.use persona, audience: "http://localhost:4000/" 
    app.use creamer,
      layout:       require "./views/layout"
      views:        __dirname + '/views'
      controllers:  __dirname + '/controllers'

    app.http.before.push do connect.cookieParser
    app.http.before.push connect.session secret: app.config.get "secret"

Let's start listening to requests from our participants:

    app.start (app.config.get "port"), ->
      app.log.info "The circles are turning at http://#{app.config.get "host"}:#{app.config.get "port"}/}"

[Vicious circle]:   http://en.wikipedia.org/wiki/Vicious_circle
[Flatiron]:         http://flatironjs.org/
[Creamer]:          https://github.com/twilson63/creamer
[Coffecup]:         https://github.com/gradus/coffeecup
[Mozilla Persona]:  http://www.mozilla.org/en-US/persona/
[Flatiron Persona]: https://github.com/lzrski/flatiron-persona
[Connect]:          http://www.senchalabs.org/connect/