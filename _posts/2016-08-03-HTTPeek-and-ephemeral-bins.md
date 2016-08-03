---
layout: post
title:  "HTTPeek: Ephemeral Bins"
date:   2016-08-03
---

I was able to merge the changes that Kevin had wanted me to make before I left, and moved on to my next story making bins ephemeral.


When a user creates a bin, they have the option of providing an expiration time in hours that is between 1 and 24. If they don't provide one the default
is  24 hours. Once a bin expires it can't be accessed by the user, and every night at midnight all expired bins are removed from the database.
This story naturally lends itself to two parts allowing users to create bins that expire, and removing expired bins from the database. I focused
on the first today, but also did some research on how to tackle the second.


The first thing I did was write a test to make verify that only unexpired bins are retrieved from the database.

```clojure

;; t refers to clj-time.core

(it "only retrieves unexpired bins"
      (let [bin (create-bin {:private false
                             :response helper/bin-response
                             :expiration (t/from-now (t/hours 24))})
            expired-bin (create-bin {:private false
                                     :response helper/bin-response
                                     :expiration (t/ago (t/hours 24))})]
        (should-contain bin (map #(get % :id) (get-bins 10)))
        (should-not-contain expired-bin (map #(get % :id) (get-bins 10))))))
```

In order to pass this test I first had to update my migrations to add a expiration field to the bins table and then update my db functions to create a
bin with an expiration time and retrieve only bins that aren't expired.

```clojure

(defn create-bin [{:keys [private response expiration] :as bin-options}]
  (-> (j/query db (str "INSERT INTO bins VALUES(DEFAULT, '" private "', '" response "', '" expiration "', DEFAULT) returning id;"))
    first
    :id))

(defn get-bins [limit]
  (j/query db (str "SELECT * FROM bins WHERE expiration > now() LIMIT " limit ";")))
```

SQL stuff can be kind of tricky, but it was surprisingly smooth. I had done some preliminary research right before I left on vacation so I'm thinking that
helped me out a bit. That was it for the db stuff. The next part was validating that the user inputted expiration time was an integer between 1 and 24.
In attempt to make things easier, I decided that I might as well use the [validateur] (https://github.com/michaelklishin/validateur) library that I used for
validating the user-inputted response fields. This took considerably more time as I misunderstood how the `:messages` and `:message-fn` options of
validateur's `numericality-of` function worked. After trying multiple things I thought should work I remembered to actually look at the source code, and eventually
came up with this.

```clojure

(defn validate-expiration [time-to-expiration]
  (let [validator (v/validation-set
                    (v/numericality-of :time-to-expiration
                                       :only-integer true
                                       :gte 1
                                       :lte 24
                                       :message-fn (fn [type _ _ & args]
                                                     (if (or (= :gte type) (= :lte type) (= :only-integer type))
                                                       "expiration time must be an integer between 1 and 24"))))]
    (v/errors :time-to-expiration (validator time-to-expiration))))
```

Here's an explanation of `:message-fn` from Validateur's source code

```clojure

":message-fn (default:nil):
               function to retrieve message with signature (fn [type map attribute & args])
               type will be one of [:blank :number :integer  :odd  :even
                                    :equal-to :gt  :gte :lt :lte]
               args will be the numeric constraint if any"

```

Next step is to handle UI portion of the validations, and then it's on to part 2, in which I will be using [quartzite] (https://github.com/michaelklishin/quartzite).
