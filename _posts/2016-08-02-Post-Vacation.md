---
layout: post
title:  "Post Vacation Thoughts"
date:   2016-08-02
---

I'm still working on HTTPeek. I got some comments on my last Pull Request before I left for vacation so much of today was spent fighting jet lag and trying
to build some context as to what it was I was actually working on pre-vacation. One thing I did when working on ebay-clone with Anda was write down current struggles
and next-steps so that we could hit the ground running in the morning. I really wish I had done the same pre-vacation. Next time...


Eventually I settled on refactoring the data-validation for user-inputted responses as well as doing a minor refactor on the javascript for a simple button.
For data-validation the two big improvements I could make were moving the actual validation into an appropriate namespace (right now I think core probably makes the most sense)
and making errors a little more user friendly. From some cryptic artifacts of our conversation from 2 weeks ago, I also gathered that Kevin wanted me to use Clojure's `ex-data`
and `ex-info` functions to help make my error messages a bit friendlier. Here's the relevant code.


```clojure
(def response-map-schema
  {(s/required-key :status) s/Int
   (s/required-key :headers) {s/Str s/Str}
   (s/optional-key :body) s/Str})

(defn- validate-response-map [body]
  (s/validate response-map-schema body))

(defn- str->vector [input]
  (if (string? input)
    (vector input)
    input))

(defn- normalize-headers [partial-header]
  (->> partial-header
    (str->vector)
   (remove empty?)))

(defn- create-headers [header-names header-values]
  (let [header-names (normalize-headers header-names)
        header-values (normalize-headers header-values)]
    (if (= (count header-names) (count header-values))
      (zipmap (map #(name %) header-names)
              (map #(name %) header-values))
      nil)))

(defn- create-bin-response [form-params]
  (let [status (edn/read-string(get form-params "status"))
        headers (create-headers (get form-params "header-name[]")
                                (get form-params "header-value[]"))
        body (get form-params "body")
        response-map {:status status
                      :headers headers
                      :body body}]
    (core/with-error-handling nil
                              (json/encode (validate-response-map response-map)))))

(defn- handle-web-create-bin [req]
  (let [form-params (:form-params req)
        private? (boolean (get form-params "private-bin"))
        bin-response (create-bin-response form-params)]
    (if-let [bin-id (core/create-bin {:private private? :response bin-response})]
      (if private?
        (-> (response/redirect (format "/bin/%s/inspect" bin-id))
          (assoc-in [:session] {:private-bins [bin-id]}))
        (response/redirect (format "/bin/%s/inspect" bin-id)))
      (-> (response/redirect "/")
        (assoc :flash "Could not create the bin, input invalid")))))
```

It might be a little hard to follow (part of the problem), but here's the gist:

When a user attempts to create a bin they must specify the HTTP response that the bin returns when a request is made to the bin's URL. The magic happens in the `handle-web-create-bin` method which takes the user-inputted
form data and parses it into a response map. The map can then be stored in the database and used for the bin. The response map must adhere to the `response-map-schema`, which is
created using the `prismatic/schema` library. If the map matches the schema then the map is returned, converted to json and then passed along to the `core/create-bin` function
If the response-map does not match the `response-map-schema` than an exception is thrown. The exception is caught by the `core/with-error-handling` macro which returns nil. Since nil is not a valid response map
the bin is not created and the user is redirected to the page, with an added generic error message.


The big issue that I failed to realize while noodling over this in the morning was that the Schema library's validate function does not provide enough context to what actually went wrong with the user's input
Was it the status, headers, or body? Who knows? It would be much better if schema provided a map of errors when the response map did not match the schema. Otherwise we are forced to do a sort of double validation.
One to test whether the schema matches and if it doesn't then another with several custom methods to see what actually went wrong  I'd imagine it'd look something like this.


```clojure
(defn- create-bin-response [form-params]
  (let [status (edn/read-string(get form-params "status"))
        headers (create-headers (get form-params "header-name[]")
                                (get form-params "header-value[]"))
        body (get form-params "body")
        response-map {:status status
                      :headers headers
                      :body body}]
    (core/with-error-handling (find-errors response-map)
                              (json/encode (validate-response-map response-map)))))

```

On second thought, it doesn't really make sense why we'd even be using schema now. We could just as easily have the `find-errors` function delegate to a host of custom validators for each piece of the response-map.
However, for such a common problem, implementing those custom validators might be a bit more error-prone and overkill. I'd imagine there must be other libraries that do this sort of validation, but better than schema.
My suspicions were confirmed at the end of the day when Myles pointed me towards one of his [old blog posts on the topic](https://8thlight.com/blog/myles-megyesi/2012/10/16/data-validation-in-clojure.html). So to recap, It seems like my issues with data validation were a case of me not recognizing that
I might be using the wrong library. I'm glad I wrote about this, and I will confirm if it's really as easy as just switching libraries tomorrow.
