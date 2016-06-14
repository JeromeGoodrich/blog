---
layout: post
title:  "Working With Sidekiq in Rails Without Active Job"
date:   2016-05-31
---

## A Quick Primer on Background Processing

Sidekiq is a background processing library for Ruby, which simply means that it allows you to schedule
certain jobs that run behind the scenes. For example, say you're writing a marketplace app that allows users
to buy and sell merchandise. When a transaction is completed you want to notify the seller that their listing
was purchased.


```ruby
  def create
    transaction = Transaction.new()
    Item.transaction do
      item = Item.lock.find(params[:item_id])
      if transaction.valid?
        transaction.save
        item.update_attributes(available: false)
        send_email(transaction, item)
        redirect_to action: "show", item_id: item.id, id: transaction.id
      else
        @presenter = NewTransactionPresenter(transaction, item)
        render "items/index", status: :unprocessable_entity
      end
    end
  end

  private

  def send_email(transaction, item)
    Mailer.confirmation_email(transaction, item).deliver_now
  end
```


Sending emails is one of the slowest tasks that a web application can do. It requires making a network
connection to an external service and then waiting for a response. You do not want the users of your app
to have to wait on an email being delivered successfully before they can go off and do something else.


Fortunately, by using Sidekiq and similar services you can schedule long-running tasks like emailing to
be handled asynchronously. To get a sense for how this works in Sidekiq, let's take a closer look at the
`send_email` method in the example above.

## Sidekiq in Practice


In our new implementation using Sidekiq let's say that the method definition for `send_email` now looks something
like this.

```ruby
  def send_email(transaction_id, item_id)
    MailerWorker.perform_async(transaction_id, item_id)
  end
```

where MailerWorker is

```ruby
class MailerWorker
  include Sidekiq::Worker

  def perform(transaction_id, item_id)
    transaction = transaction.find(transaction_id)
    item = Item.find(item_id)
    Mailer.confirmation_email(transaction, item).deliver_now
  end
end
```


All that we've done, is wrap the `Mailer.confirmation_email` method in the mailer worker's perform task, which
is necessary for a Sidekiq worker to be processed. Another thing you may have noticed is that instead of passing
in the transaction and item objects directly to the `perform` method we pass in a their ids and then query the
database for the actual record. This is due to the in-memory data-structure-store that Sidekiq uses called Redis.



## Redis and Serialization

In the example above, The database I am querying with `Item.find(item_id)` is Postgres, a relational database.
You can think of Postgres and other relational databases as just a bunch of tables that represent the type of object
you are trying to store. The rows of the tables represent individual records for the object the table represents and
and the columns represent that object's attributes. Redis, is a key-value database so it works a bit differently. Instead
of having nicely laid out structure in the form of tables, a key-value database like Redis relies on storing information
in hashes/dictionaries. This makes it much more flexible as each record can have different fields, unlike a relational database.

You don't want to send things to Redis that can change state. Furthermore the Sidekiq API uses JSON.dump to send things to Redis,
and JSON.load to get them back. both of these only work with simple JSON datatypes like strings, integers, arrays, etc.

## Apologies and More Resources

This deep dive has taken longer than anticipated, and I'm not really sure where I'm going with this anymore.
Here is a [great article on getting started with Sidekiq][1] from someone much more eloquent and articulated than me.


[1]: http://www.garrettqmartin.com/2014/08/17/background-jobs-with-sidekiq.html
