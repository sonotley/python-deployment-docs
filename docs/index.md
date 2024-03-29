# Introduction

> This site is intended as a narrative on how and why I devised the _snoap_ workflow. 
> It's not a complete definition of the workflow or documentation for it.
> If that's what you're looking for check out [cookiecutter-snoap](https://github.com/sonotley/cookiecutter-snoap) 
> which is kind of reference implementation for snoap and probably gets updated more than this site does.

## What is this all about?
Python is pretty easy to write, but when it comes to packaging, distributing and running code somewhere other than your own computer, things get a bit tricky. This is a guide to how I've chosen to tackle that problem. It's a reference for my future self and hopefully of help to other people too.

## Who is this for?
In general, I have two target use-cases when I'm writing code, if you have similar needs perhaps you'll find this site useful.

- I'm providing a utility for users to use on their workstations
- I'm proving software to be run as a service on a server

When I started this project, these workstations and servers were often Windows, and that has shaped my approach to some extent. 
For example, I have a preference for keeping everything in a single directory windows-style.

The two use-cases share some similarities:

- I can't guarantee the version of Python (or even the existence of Python) on the target system
- I don't want the user to need to know any Python or to do lots of setup work, it needs to just install and work
- I don't want the deployed code to be scattered around non-obvious places, ideally everything should be in one directory so it can be uninstalled simply by deleting that directory

In the case of deployment to a server there is an important addition to the third point above:

- The installed software must not be dependent on the continued existence of the user account by which it was installed.


## Who is this _not_ for?
Well, I like to think anyone with an interest in Python could get something from these pages, but there are a few area I'm not planning on addressing simply because they aren't options I generally use. They aren't bad options, they just aren't part of the environment I'm currently working in. They are:

- Deploying software as a Docker container
- Deploying to 'serverless' cloud services such as Google App Engine, AWS Lambda, Heroku