---
layout: post
title: Heroku and Migration
categories: rails heroku migration database
---

In heroku when you relase a new version with you have to remember the migration.
  
    git push heroku master && heroku run rails db:migrate

It's easy to forget the second part and the application fail only after someone open a page.  
With a new (3 august 2016) feature of Heroku (https://devcenter.heroku.com/articles/release-phase) inside Procfile you can easily fix this problem with a single line:

    # Procfile
    release: bundle exec rails db:migrate