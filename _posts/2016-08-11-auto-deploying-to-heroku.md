---
layout: post
title: HTTPeek - Auto-deploying To Heroku With Travis CI
date: 2016-08-11
---

For the uninitiated deploying a clojure app to heroku can be a time consuming endeavor. Here are some of the things I learned while deploying
HTTPeek.


  - Heroku can be picky about the configuration of your app. Make sure to read Heroku's [clojure support documentation](https://devcenter.heroku.com/articles/clojure-support)
    carefully. I ended up needing to change a couple of things to make it work.

  - HTTPeek uses a postgres database, and Heroku makes it easy to create one for the newly deployed app: `heroku addons:create heroku-postgresql:hobby-dev`

  - Once a database is provisioned the only thing left is to create the tables and conduct any necessary migrations. For
    HTTPeek I created a leiningen alias for my migrations. So migrating my tables is as simple as `heroku run lein migrate`.
    Well it would be once the app is configured correctly. Heroku uses a "DATABASE_URL" environment variable to create a
    connection with the database. "DATABASE_URL" is a string with the following structure `<db-type>//<username>:password@<host>:<port>/<db-name>`
    Before Heroku this is how HTTPeek configured it's databases:

```clojure
{:dev {:env {:db-classname "com.postgresql.jdbc.Driver"
            :db-subprotocol "postgresql"
            :db-subname "//localhost:5432/httpeek"
            :db-password ""
            :db-user "admin"}}
:test {:env {:db-classname "com.postgresql.jdbc.Driver"
             :db-subprotocol "postgresql"
             :db-subname "//localhost:5432/httpeek_spec"
             :db-password ""
             :db-user "admin"}}}
```

And after Heroku


```clojure

{:dev {:env {:database-url "postgres://httpeek:password@localhost:5432/httpeek"}}
 :test {:env {:database-url "postgres://httpeek:password@localhost:5432/httpeek_spec"}}
 :heroku {:env {:database-url #=(eval (System/getenv "DATABASE_URL"))}}}
```

`clojure.jdbc` will take a map as well as a plain string to create a database connection.


With the database configured correctly, we can migrate and once the migrations finish HTTPeek runs perfectly, But there is a
significant improvement that can be made to this process, namely that a large portion of it can be automated using a tool like
Travis CI. Since HTTPeek is already using Travis this equates to a couple of small changes to the `.travis.yml` file. You can avail
yourself of the documentation to get things set-up or you can use the [Travis commandline tool](https://github.com/travis-ci/travis.rb)
to take care of the set-up for you. It's as easy as `travis setup heroku`. You can even configure the build to run the migrations for you
with the `run:` key. Now every time you push changes to your app and your build passes it will also deploy to Heroku!
