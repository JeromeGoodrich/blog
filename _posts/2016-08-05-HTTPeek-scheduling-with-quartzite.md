---
layout: post
title: "HTTPeek: Scheduling with Quartzite"
date: 2016-08-05
---

Yesterday I started exploring how I might go about scheduling deletion of expired bins. I decided to use [quartzite](http://clojurequartz.info/) and ended up with the following
code at the end of the day yesterday:


```clojure

(defjob DeleteExpired [_]
  (db/delete-expired-jobs))

(defn delete-expired-bins []
  (let [s (-> (qs/initialize) qs/start)
        job (j/build
              (j/of-type DeleteExpired))
        trigger (tg/build
                  (tg/start-now)
                  (tg/with-schedule
                    (schedule
                      (with-interval-in-days 1)
                      (every-day)
                      (starting-daily-at (time-of-day 00 00 00)))))]
    (qs/schedule s job trigger)))
```

The problem with the code above is that I have no idea whether it works or not. That to me is a smell that I might be able to refactor this and make it more testable.
Kevin said as much, and suggested that I make a function that handles the configuration of this scheduling, make the specifics paramatizable. Then at least I could check
That I'm scheduling with the correct configuration, and change the interval to run more frequently whcih would allow me to test things locally.
