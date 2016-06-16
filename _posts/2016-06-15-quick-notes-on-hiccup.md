---
layout: post
title:  "Quick Notes on Hiccup"
date:   2016-06-15
---

Hiccup is an HTML templating library for clojure. It's light-weight and dead simple. In Hiccup HTML tags
are just Clojure vectors. There's no templates, or interpolation, just pure Clojure. Each html node has only 3 parts.

1. a tag
2. attributes (optional)
3. contents (optional, any number)

```clojure
[:a {:href "http://www.example.com} "This is an example"]
```

that renders

```html
<a href="http://example.com"> This is an example </a>
```


If you want to nest elements just include the nested nodes as the contents of the parent node.

```clojure
[:ul {:class "Warriors"}
 [:li "Steph"]
 [:li "Draymond"]
 [:li "Klay"]]
```

It's that simple.

Since Hiccup is just plain old clojure vectors, they can be manipulated like any other
Clojure code.

```clojure
(defn todo-list
  "transform a clojure list of todo items into a unordered html list"
  [items]
  [:ul {:class "to-do"}
    (for [item items]
    [:li item])])

(grocery-list ["pay rent" "pick-up dry cleaning"])
;; -> [:ul {:class "groceries"} ([:li "pay rent"] [:li "pick-up dry cleaning"])]

```
