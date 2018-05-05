---
layout: post
title: Heroku and migration for Rails
categories: rails
---

In [Heroku](http://heroku.com) when you relase a new version with Rails, you have to remember the migration for the database:

    git push heroku master && heroku run rails db:migrate

It's easy to forget the second part and the application fail only after someone open a page.  
With a new [feature of Heroku](https://devcenter.heroku.com/articles/release-phase) (3 August 2016) inside Procfile you can easily fix this problem with a single line:

    # Procfile
    release: bundle exec rails db:migrate
