---
layout: page
title: Résumé
---

## Tools and Technologies I use and love 😍

| Area       | Technology (most fluent first)                        |
| ---------- | ----------------------------------------------------- |
| Languages  | Ruby, Javascript (es2015), Elixir, PHP & Scala        |
| Frameworks | Rails, Sinatra, React, Flux, Ember & Backbone         |
| Databases  | Postgres, MySQL, Redis, Elasticsearch, MongoDB        |
| Testing    | Rspec, MiniTest, Jasmin, ExUnit                       |
| Tools      | Git, Github, Vim                                      |

## Work History

### [Flatmates.com.au](https://flatmates.com.au) - (Sydney, Australia)
#### Software Engineer - (2016-Current)

Flatmates.com.au is Australia’s No.1 share accommodation website. Based in Sydney, Australia, Flatmates.com.au allows people to list their spare rooms, find accommodation or team up with others to start a share house.

As a developer at Flatmates I work on all parts of the web app. From user interface to server management in AWS. We're a small team running a fairly busy site so being comfortable with all parts of our stack is critical. It's not uncommon for a work day to include refactoring SASS and updating chef scripts.

 - Ruby on Rails
 - OpsWorks
 - Postgres, Elasticsearch, Redis
 - Sidekiq
 - ReactJS, Flux
 - ActiveMerchant
 - Sass, neat and bourbon
 - Rspec

#### Highlights

 - Rebuilt the front end of our messaging system from scratch using React, ReactRouter (v4) and Flux. Improving compatibility and performance on mobile devices while reducing CSS.
 - Ported our search system form giant controller methods to small testable service objects.
 - Built an emailing system to help members advertising properties find relevant matches. Our email system is our largest source of revenue and this became the 3rd largest source.
 - Built the [search by maps](https://flatmates.com.au/rooms/enmore-2042) feature using React and dynamic SVG’s for markers.
 - Swapped out the Google Places Autocomplete with our own [Autocomplete built in React](https://flatmates.com.au/value-my-room/enmore-2042) and Flux using Elasticsearch as a backend.
 - Implemented zero downtime deploys using ELB and Puma.
 - Migrated our Elasticsearch from being third party hosted into AWS with signed requests for better security.
 - Helped move the team to an **Kanban** based workflow.

### [YourTutor](http://yourtutor.com.au) (Sydney, Australia)
##### Software Engineer (2013-2016)

YourTutor provides an on-demand online tutoring tool connecting qualifieds tutors with primary school, high school and university students in Australia.

The engineering team comprises a team of full stack engineers (mostly remote), a designer, product manager and team lead. The team followed an agile-ish methodology with two weeks spreets with people rotating into different roles each sprint to help share knowledge and keep things interesting. As the team had no dedicated Dev Ops or Sys Admin roles developers need to be involved in managing servers and deploying code.

- Ruby On Rails, EventMachine, Socket.io
- EmberJs, ReactJs, Backbone, EaselJs, RaphaelJs, CoffeeScript
- Postgres, Redis
- Ansible
- Sass

#### Highlights

- Re-designed our EventMachine servers to allowing it scale horizontally horizontally using Redis as a queueing backend.
Rolled out A/B testing frameworks (split) to help us perform multi-variant testing mostly for features around acquisition (pricing, sign up flow etc).
Upgraded our main Rails app from 3.2 to 4.x making sure we continue to keep up to date so as to avoid big ugly upgrades.
- Built a load testing/smoke testing tool to run against production after deploys. I used PhatomJS and poltergeist to simulate a large number of tutors and students logging onto our system and connecting to each other.
- Helped lead the efforts into using service objects, view models and form objects to help organise and unit test a large app.
- Lead the implementation ReactJs components as a move away from backbone.
- Helped increase the reliability and frequency of our deploys by formalising out github pull request workflow in documentation.
- Researched and tuned Ruby GC settings after seeing unbounded memory growth when moving to ruby 2.1.
- Improving our overall test coverage and my understanding of TDD and testing in general with things like Rspec 3 and verified mocks.
- Helped organise our large mono-rail app into modules and introduced the use of service objects (even though “service” is a poor name)

### [LawPath](http://lawpath.com.au) (Sydney, Australia)
##### Software Engineer (2013-2013)

LawPath is a online lead generation tool that helped connect consumers and businesses seeking legal advice to relevant lawyers in their area.

As a software engineer at LawPath I helped build and maintain there Rails application and helped implement testing, code review, continuous integration and continious deployment.

### [Squiz](http://squiz.com.au) (Sydney, Austrlia)
##### Production Software Engineer (2010-2013)

Squiz is an Australian Supported Open Source Solutions company. They provide services around a suite of open source web applications including CMS, Search and Analytics. All products are built using open source technologies PHP, Postgres and MySQL.

As a Production Software Engineer at Squiz I was manly responsible for the development and maintanance of a older PHP and MySQL CMS platform for a high value client that required a very customised system that served and consumed many API's over SOAP and REST

## Formal Education
### Royal Melbourne Institute of Technology (Melbourne, Australia)
##### Bachelor in Computer Science (Applied Science)
##### Completed 2006

Degree Focus: Object-Oriented programming, Distributed Systems, Artificial Intelligence and System Administration. Concurrent development done in C, C++ and Java.

---
[PDF Version](https://www.dropbox.com/s/3ry3fv04mi3mjja/HanLoongLiauw2017.pdf?dl=0)
