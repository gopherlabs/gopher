# Gopher
> Gopher is a modern, light weight, and extensible web framework for Go

[![Build Status](https://travis-ci.org/gopherlabs/gopher.svg)](https://travis-ci.org/gopherlabs/gopher)
[![License](http://img.shields.io/:license-mit-blue.svg)](http://doge.mit-license.org)
[![Coverage Status](https://img.shields.io/coveralls/gopherlabs/gopher.svg)](https://coveralls.io/r/gopherlabs/gopher)
[![GoDoc](https://godoc.org/github.com/gopherlabs/gopher?status.svg)](https://godoc.org/github.com/gopherlabs/gopher)
[![Made with heart](https://img.shields.io/badge/made%20with-%E2%99%A5-orange.svg)](https://github.com/ricardo-rossi)
[![Join the chat at https://gitter.im/gopherlabs/gopher](https://img.shields.io/badge/GITTER-join%20chat-green.svg)](https://gitter.im/gopherlabs/gopher?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge)

```go
Route.Get("/", func(w http.ResponseWriter, r *http.Request) {
    Render.Text(w, "Hello, Gopher!")
})
ListenAndServe()
```
```awk
> go run server.go

INFO[0000] |----------------------------------------|   
INFO[0000] |    _____                                   
INFO[0000] |   / ____|           | |                    
INFO[0000] |  | |  __  ___  _ __ | |__   ___ _ __       
INFO[0000] |  | | |_ |/ _ \| '_ \| '_ \ / _ \ '__|      
INFO[0000] |  | |__| | (_) | |_) | | | |  __/ |         
INFO[0000] |   \_____|\___/| .__/|_| |_|\___|_|         
INFO[0000] |               | |                          
INFO[0000] |               |_|                          
INFO[0000] |----------------------------------------|   
INFO[0000] | GOPHER READY FOR ACTION ON PORT 3000             
INFO[0000] |----------------------------------------|   
```
```awk
> curl http://localhost:3000/
Hello, Gopher!
```

**Want live code reload?**
Use either [gin](https://github.com/codegangsta/gin) or [fresh](https://github.com/pilu/fresh) to live 
reload Gopher apps.

## Table of Contents

* [Overview](#overview)
* [Heroic Features](#heroic-features)
* [Getting Started](#getting-started)
* [The Basics](#the-basics)
  * [Routing](#routing)
    * [Routing Overview](#routing-overview)
    * [Request Handlers](#request-handlers)
    * [Route Verbs](#route-verbs)
    * [Multiple Verbs](#multiple-verbs)
    * [Route Variables](#route-variables)
    * [Route Groups](#route-groups)
    * [Serving Static Files](#serving-static-files) 
    * [Not Found Route](#not-found-route)
    * [Routing Configuration](#routing-configuration)
  * [Middleware](#middleware)
    * [Middleware &amp; the Request-Response Lifecycle](#middleware--the-request-response-lifecycle)
    * [Application-level Middleware](#application-level-middleware)
    * [Router-level Middleware](#router-level-middleware)
    * [RouteGroup-level Middleware](#routegroup-level-middleware)
    * [Route-level Middleware](#route-level-middleware)
    * [Built-in Middleware](#built-in-middleware)
  * [Context](#context)
  * [Logging](#logging)
    * [Logging Overview](#logging-overview)
    * [Log Levels](#log-levels)
    * [Logging Configuration](#logging-configuration)
  * [Views &amp; Templates](#views--templates)
  * [Responses](#responses)
[]([Architecture](#architecture)[IoC Container](#ioc-container)[Contracts](#contracts)[Facades](#facades)[Service Providers](#service-providers))  
[]([Advanced Topics](#advanced-topics)[Creating Service Providers](#creating-service-providers)[Custom Bootstrapping](#custom-bootstrapping)[API Documentation](#api-documentation)[Performance &amp; Benchmarks](#performance--benchmarks))
* [Roadmap](#roadmap)
* [Frequently Asked Questions](#frequently-asked-questions)
* [Support](#support)
* [Contribution Guide](#contribution-guide)
* [Authors](#authors)
* [License](#license)

## Overview 

//TODO

## Heroic Features

* **Simple**: Straightforward, clean *Idiomatic* Go syntax. 
* **Intuitive**: Beautiful APIs for maximum coding happiness and productivity.
* **Exposed**: No reflection, dependency injection, or hidden magic. Just clean Go interfaces.
* **Modern**: Features an IoC Container, nested Middleware, flexible Routing, and more.
* **Extensible**: Easy to add Service Providers or even replace the built-in ones.
* **Comprehensive**: Routing, Handlers, Middleware, Logging, Views, and much more.
* **Speedy**: Gopher is blazing fast. See our benchmarks.
* **Documented**: Thoroughly detailed APIs

## Getting Started

Let's create our first "Hello, Gopher!" example.

#### 1. Install Gopher

1. [Install Go](https://golang.org/dl/) and set your [GOPATH](http://golang.org/doc/code.html#GOPATH) (if you haven't already).
2. Then, from your GOPATH, type this to install Gopher and its dependencies:

```
go get github.com/gopherlabs/gopher
```

#### 2. Create your server.go file

```go
package main

import (
	"net/http"
	. "github.com/gopherlabs/gopher"
)

func main() {
	Route.Get("/", func(w http.ResponseWriter, r *http.Request) {
		Render.Text(w, "Hello, Gopher!")
	})
	ListenAndServe()
}
```
See this example at: [routes/01_hello.go](https://github.com/gopherlabs/gopher-examples/blob/master/routes/01_hello.go)

> **NOTE:** Only for the purpose of syntax clarity, the example above uses the dot import notation as in: 
`import . "github.com/gopherlabs/gopher"` It should be noted, however, that the Go team does not recommend 
using the dot import since it can cause some odd behaviour in certain cases.

#### 3. Run your server

```shell
go run server.go
```

You will now have a Gopher web server running on localhost:3000 (default port) and you should 
see the following output:

```awk
INFO[0000] |----------------------------------------|   
INFO[0000] | LOADING SERVICE PROVIDERS ...              
INFO[0000] |----------------------------------------|   
INFO[0000] | * LOGGER ✓                                 
INFO[0000] | * MAPPER ✓                                 
INFO[0000] | * ROUTER ✓                                 
INFO[0000] | * RENDERER ✓                                                     
INFO[0000] |----------------------------------------|   
INFO[0000] |    _____                                   
INFO[0000] |   / ____|           | |                    
INFO[0000] |  | |  __  ___  _ __ | |__   ___ _ __       
INFO[0000] |  | | |_ |/ _ \| '_ \| '_ \ / _ \ '__|      
INFO[0000] |  | |__| | (_) | |_) | | | |  __/ |         
INFO[0000] |   \_____|\___/| .__/|_| |_|\___|_|         
INFO[0000] |               | |                          
INFO[0000] |               |_|                          
INFO[0000] |----------------------------------------|   
INFO[0000] | GOPHER READY FOR ACTION ON PORT 3000             
INFO[0000] |----------------------------------------|   
 
```

#### 4. Now, try it!

```awk
> curl http://localhost:3000/
Hello, Gopher!
```
Awesome, it worked!   

Next, let's take a look at some of the basic concepts of Gopher:

# The Basics

## Routing

#### Routing Overview

You will define the routes for your application using the Route instance, which is satisfies the 
[*Routable*](https://godoc.org/github.com/gopherlabs/gopher-framework#Routable) interface. 

The most basic Gopher routes simply accept a URI and a Closure as in:

```go
Route.Get("/", func(w http.ResponseWriter, r *http.Request) {
  Render.Text(w, "Hello, Gopher!")
})
```

#### Request Handlers

Although you can use Closures as in the example above, it is often more practical to encapsulate 
the request handling logic in handler functions which can be reused between routes:

```go
func main() {
	Route.Get("/hello", HelloHandler)
	ListenAndServe()
}

func HelloHandler(w http.ResponseWriter, r *http.Request) {
	Render.Text(w, "Hello, Handler!")
}
```

#### Route Verbs

Gopher provides routing methods to handle every specific http verb: 

```go
func main() {
	Route.Get("/products", VerbHandler)
	Route.Post("/form", VerbHandler)
	Route.Put("/update", VerbHandler)
	Route.Delete("/etc", VerbHandler)
	Route.Head("/etc", VerbHandler)
	Route.Options("/etc", VerbHandler)
	ListenAndServe()
}

func VerbHandler(w http.ResponseWriter, r *http.Request) {
	Render.Text(w, "Hello, "+r.Method)
}
```

#### Multiple Verbs

Sometimes you may need to register a route that responds to multiple HTTP verbs. You may do so using 
the [*Match()*](https://godoc.org/github.com/gopherlabs/gopher-framework#RouteFacade.Match) method of 
the [Route Facade](https://godoc.org/github.com/gopherlabs/gopher-framework#RouteFacade) as shown on 
this example:

```go
Route.Match("/hello", MatchHandler, []string{"GET", "POST", "PUT"})
```

Or, you may even register a route that responds to all HTTP verbs using the 
[*All()*](https://godoc.org/github.com/gopherlabs/gopher-framework#RouteFacade.All) method:

```go
Route.All("/", AllHandler)
```

#### Route Variables

Sometimes you will need to capture segments of the URI within your route. You may do so by 
defining route variables. Route variables are always encased within "curly" braces. They are 
defined using the format *{name}* or *{name:pattern}*

For example, you may need to capture a user's ID from the URL:
```go
Route.Get("user/{id}", func(w http.ResponseWriter, r *http.Request) {
  Render.Text(w, "User: "+Route.Var(r, "id"))
})
```

You may define as many route parameters as required by your route:

```go
Route.Get("posts/{post}/comments/{comment}", func(w http.ResponseWriter, r *http.Request) {
  // The entire map of route variables can be retrieved calling Route.Vars()
})
```

#### Route Groups

Route groups allow you to share route attributes (such as path prefixes, hosts, methods, etc)  across a 
large number of routes without needing to define those attributes on each individual routes. 

```go
RouteGroup.New(matcher GroupMatcher) Routable
```

Shared attributes are passed as type `GroupMatcher` as the first parameter to `RouteGroup.New()`.

```go
type GroupMatcher struct {
	Host       string
	PathPrefix string
	Methods    []string
	Queries    []string
	Schemes    []string
}
```

**Route Prefixes**

The `PathPrefix` attribute may be used to prefix each route in the group with a given URI. 
For example, you may want to prefix all route URIs within the group with */products*:

```go
group := RouteGroup.New(GroupMatcher{
  PathPrefix: "/products",
})

group.Get("/item", func(rw http.ResponseWriter, req *http.Request) {
  Render.Text(rw, "Hello Item!")
})
```

In this example, the */item* route inherits all the route attributes of its parent *RouteGroup*, such as 
its path prefix:

```awk
> curl http://localhost:3000/products/item
Hello Item!
```


#### Serving Static Files

Serving files, such as images, CSS, JavaScript and other static files is accomplished with the help 
of the `Route.Static()` API. 

```go
Route.Static(path string, dir string)
```

Where:

* `path` is path prefix for the files served by Gopher.
* `dir` the name of the directory, which is to be used as the location of static assets.

For example, if you keep your images, CSS, and JavaScript files in a directory named public, you can do this:

```go
Route.Static("/static", "./public")
```

Now, you will be able to load the files under the public directory, from the path prefix "/static".

```
http://localhost:3000/static/images/kitten.jpg
http://localhost:3000/static/css/style.css
http://localhost:3000/static/js/app.js
http://localhost:3000/static/images/bg.png
http://localhost:3000/static/hello.html
```

#### Not Found Route

Using `Route.NotFound()`, you may register an error handler that handles all "404 Not Found" errors 
in your application, allowing you to easily return custom 404 error pages, or execute any code you want. 

```go
Route.NotFound(func(rw http.ResponseWriter, req *http.Request) {
  Render.Text(rw, "Could not find page")
})
```

#### Routing Configuration

**How do I change the port/host?**

Gopher's *ListenAndServe()* function defaults to using HOST value of 0.0.0.0 and PORT 3000. 

An easy way to change those values is to set PORT and HOST environment variables before running 
Gopher like this: 

```awk
PORT=8080 HOST=localhost go run server.go
```

```awk
> curl http://localhost:8080/
Hello, Gopher!
```

If you don't want to set the PORT and HOST environment variables you can also configure those values 
using the *App.Config()* API as shown below:

```go
App.Config(Config{
  KEY_ROUTER: ConfigRouter{
    Port: 8080,
    Host: "localhost",
    StaticDirs: map[string]string{
      "/static": "./static/",
    },
  },
})
```

Otherwise, if you want more flexibility over the way you start your app, use the *GetHttpHandler()* function 
instead, which returns the built-in [http.Handler](https://godoc.org/net/http#Handler):

```go
http.ListenAndServe("localhost:8080", GetHttpHandler())
```

## Middleware

In Gopher, a Middleware is a function with access to the request object (*http.Request*), the response 
object (*http.ResponseWriter*), the next middleware in the application's request-response cycle, 
commonly denoted by a function argument named next, which accepts a variadic number or arguments satisfying
the *MiddlewareHandler* type.

```go
func(w http.ResponseWriter, r *http.Request, next func(), args ...interface{})
```

Gopher Middleware can:

* Execute any code.
* Make changes to the request and the response objects.
* End the request-response cycle.
* Call the next middleware in the stack.

If the current middleware does not end the request-response cycle, it must call *next()* to pass control to 
the next middleware, otherwise the request will be left hanging.

A Gopher application can use the following kinds of middleware:

* [Application-level Middleware](#application-level-middleware)
* [Router-level Middleware](#router-level-middleware)
* [RouteGroup-level Middleware](#routegroup-level-middleware)
* [Route-level Middleware](#route-level-middleware)
* [Built-in Middleware](#built-in-middleware)

#### Middleware &amp; the Request-Response Lifecycle 

//TODO

#### Application-level Middleware

//TODO

#### Router-level Middleware

#### RouteGroup-level Middleware

//TODO

#### Route-level Middleware

//TODO

#### Built-in Middleware

//TODO

## Context

```go
Context.Set("user", "Ricardo")
Route.Get("/user", func(w http.ResponseWriter, r *http.Request) {
  Render.Text(w, "Hello, "+Context.Get("user").(string))
})
```

```go
type Mappable interface {
	Get(key string) interface{}
	Has(key string) bool
	Set(key string, value interface{})
	Remove(key string)
}
```

## Logging

#### Logging Overview

Gopher has six logging levels: Debug, Info, Warning, Error, Fatal and Panic:

```go
Log.Debug("Useful debugging information.")

Log.Info("Something noteworthy happened!")

Log.Warn("You should probably take a look at this.")

Log.Error("Something failed but I'm not quitting.")

// Calls os.Exit(1) after logging
Log.Fatal("Bye.")

// Calls panic() after logging
Log.Panic("I'm bailing.")
```

#### Log Levels

You can set the logging level on using the global *App.Config()* API, so it will only log entries with 
that severity or anything above it:

```go
App.Config(Config{
  KEY_LOGGER: ConfigLogger{LogLevel: LEVEL_INFO},
})

// Debug logs will not be logged since we set the Log Level to LEVEL_INFO
Log.Debug("Useful debugging information.")

// Anything with severity Info or above it will be logged
Log.Info("Something noteworthy happened!")

Log.Warn("You should probably take a look at this.")

Log.Error("Something failed but I'm not quitting.")
```

#### Logging Configuration 

Besides configuring the log level, you can also specify the time stamp format for the logging 
output by setting the *ConfigLogger.TimestampFormat* attribute as shown below:

```go
App.Config(Config{
  KEY_LOGGER: ConfigLogger{
    TimestampFormat: time.RFC822,
    LogLevel:        LEVEL_INFO,
  },
})
```

## Views &amp; Templates

//TODO

## Responses

//TODO

[]([Architecture](#architecture)[IoC Container](#ioc-container)[Contracts](#contracts)[Facades](#facades)[Service Providers](#service-providers))  

## Roadmap

#### v0.9 

- [x] Routing APIs
- [x] Request Handlers  
- [x] Nested Middleware 
- [x] Application Context
- [x] Logging
- [x] Views &amp; Templates
- [x] Responses
- [x] IoC Container
- [x] Contracts  
- [x] Facades 
- [x] Initial Documentation  

#### v1.0 

- [ ] Extensibility APIs
- [ ] Performance Benchmarks 
- [ ] Enhanced Documentation  
- [ ] Enhanced Test Cases  

#### v2.0 

- [ ] DB/ORM 
- [ ] Mail
- [ ] Sessions 
- [ ] Caching
- [ ] Hashing
- [ ] Authentication
- [ ] Queues? 

#### v3.0 

- [ ] Micro-Gopher Docker Containers
- [ ] Health &amp; Service Instrumentation
- [ ] LTS Support Options

## Frequently Asked Questions

* **This looks great and I would like to start using Gopher right now, but how stable is it?** 

   Gopher is currently in alpha (pre v0.9 release) but by all means use it now! We don't have any planned breaking API changes 
   (although we can't guarantee it won't happen) from now until the the v1.0 release as we are mainly focusing on performance, 
   testing, bug fixes, and extensibility until then.  

* **Is anyone using Gopher yet?** 

   At Gopher Labs, we are using Gopher for several customer projects. If you, or anyone you know is using Gopher please let us
   know and we can include them here. 

* **Will you provide Long-term support (LTS)?** 

   Yes, LTS options are planned for Gopher v3 (Target Release H1 2016). Please see our [Roadmap](#roadmap) for details.

## Support

**Forum:** https://groups.google.com/d/forum/gopher-framework

**Mailing List:** gopher-framework@googlegroups.com

**Twitter:** For Gopher announcements, follow us at [@gopherweb](https://twitter.com/gopherweb) 

**Chat:** Join the conversation at [![Join the chat at https://gitter.im/gopherlabs/gopher](https://img.shields.io/badge/GITTER-join%20chat-green.svg)](https://gitter.im/gopherlabs/gopher?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge)

## Contribution Guide

Thank you for considering contributing to Gopher!

To encourage active collaboration, we strongly encourage [pull requests](https://github.com/gopherlabs/gopher/pulls), 
not just bug reports. "Bug reports" may also be sent in the form of a 
pull request containing a failing test.

However, if you file a bug report, your [issue](https://github.com/gopherlabs/gopher/issues) should contain a title 
and a clear description of the problem. You should also include as much relevant information as possible 
and a code sample that demonstrates the issue. The goal of a bug report is to make it easy to replicate 
the bug and develop a fix.

#### Opening a Pull Request

1. Fork the appropriate Gopher repository: [gopher](https://github.com/gopherlabs/gopher), 
[gopher-framework](https://github.com/gopherlabs/gopher-framework), 
[gopher-services](https://github.com/gopherlabs/gopher-services)
2. Create your feature branch (`git checkout -b my-new-feature`)
3. Commit your changes (`git commit -am 'Add some feature'`)
4. Push to the branch (`git push origin my-new-feature`)
5. Create new [Pull Request](https://github.com/gopherlabs/gopher/pulls)

## Authors 

Created with ♥ by [Ricardo Rossi](https://github.com/ricardo-rossi) 
[@ricardojrossi](https://twitter.com/ricardojrossi), founder of Gopher Labs ✉ ricardo@gopherlabs.org

Maintained with care by all its [Contributors](https://github.com/gopherlabs/gopher/graphs/contributors) 

Built-in Service Providers include the following vendored projects:

  * gorilla/mux [gorilla/mux](https://github.com/gorilla/mux)
  * sirupsen/logrus [https://github.com/sirupsen/logrus](https://github.com/sirupsen/logrus)
  * unrolled/render [https://github.com/unrolled/render](https://github.com/unrolled/render)

## License 

The Gopher framework is open-sourced software licensed under the [MIT license](mit-license.md)

Copyright (c) 2015 Ricardo Rossi (Gopher Labs) - ricardo@gopherlabs.org 


## Thank you for using Gopher!

We hope you enjoy using Gopher. Please Star it, Watch it, &amp; Share this repo.

Follow us on Twitter [@gopherweb](https://twitter.com/gopherweb) 

```
   _____             
  / ____|           | |                                	                        ..   ..   . ..
 | |  __  ___  _ __ | |__   ___ _ __                   	                 .  .?8MMMMMNOZ$ZONMMMN~..
 | | |_ |/ _ \| '_ \| '_ \ / _ \ '__|                  	            .   8M8~.                   .OMI. . ... .
 | |__| | (_) | |_) | | | |  __/ |                     	    . .   ...MD    . ..          .    ..    7M ?MMMMMO
  \_____|\___/| .__/|_| |_|\___|_|                     	   MM:. :DNM ...MI.. 8Z.         +I.   .. N   :M      ?M
              | |                                      	 :M .    M..  ?.       .M       $         . =  .M ~,   ?.
              |_|                                      	 M.  .7IM   .            $.   .7 .               MMM.   8.
  _    _      _                                        	.M   8MM    +.....        M   ~ =MMN         D.   MM   .I.
 | |  | |    | |                                       	.M   .Z:   ~ MMMMN.           Z+MMMMM.       D     M  .M.
 | |  | | ___| |__                                     	. M  .M.   . MMM.M        ~     MMMII.       D     Z,M7
 | |/\| |/ _ \ '_ \                                    	  .OMO$.   .O,MMN        .M    D.  .              . M..
 \  /\  /  __/ |_) |                                   	     M      .8.          M .++:.O .        $.       M .
  \/  \/ \___|_.__/                                    	     M       .D .     .=, MMMMMM. 77. .. M          ?7
 ______                                           _    	   .,8          .8MNN7  ,MMMMMMO8?. ... .           .M
 |  ___|                                         | |   	   .$=                 M .. ...  ..                  M
 | |_ _ __ __ _ _ __ ___   _____      _____  _ __| | __	    N                  N    .  . . =                 M
 |  _| '__/ _` | '_ ` _ \ / _ \ \ /\ / / _ \| '__| |/ /	   .M.                  $OO..N ?MD=                  M
 | | | | | (_| | | | | | |  __/\ V  V / (_) | |  |   < 	    M                   . .  7  N                    M
 \_| |_|  \__,_|_| |_| |_|\___| \_/\_/ \___/|_|  |_|\_\	   .M.                  . . .N  N                    M.

```
