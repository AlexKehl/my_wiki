# Malli

A schema validation library for clojure.

## Schema definition

````clojure
(ns my-clojure-project.core
  (:require [malli.core :as m]))

(def UserSchema
  [:map [:id int?]
   [:name string?]
   [:age {:min 18} int?]
   [:email string?]
   [:password string?]])

### Validation
```clojure
(m/validate UserSchema
            {:id       1
             :name     "John"
             :age      30
             :email    "foo@bar.com"
             :password "secret"})
````

Returns true for valid and false for invalid
For error explanation (and better readability) use

```clojure
(let [result (m/explain UserSchema (get-user "50"))]
  (def result result)
  (if result
    (do (println "Validation failed:") (println (me/humanize result)))
    (println "Validation passed")))
```

## Sample generation / testing

Generate samples like

```clojure
(gen/sample (mg/generator UserSchema) 5)
```

Test like

```clojure
(ns my-clojure-project.controller.user-test
  (:require [clojure.test.check.clojure-test :as tcct]
            [clojure.test.check.properties :as prop]
            [malli.core :as m]
            [malli.generator :as mg]
            [my-clojure-project.controller.user :refer [get-user UserSchema]]))

(tcct/defspec test-get-user
              100
              (prop/for-all [id (mg/generator int?)]
                            (m/validate UserSchema (get-user id))))
```

## Record like syntax (homogeneours maps)

```clojure
(m/validate [:map-of :string :int] {"foo" 1 "bar" 2})
```

similar to `Record<string, number>` in typescript

## Humanized error messages

Humanized error messages can be customized and also support localization

```clojure
(ns my-clojure-project.lib.json-schema
  (:require [malli.error :as me]))

(def UserId :string)

(def Address [:map [:street :string] [:country [:enum "FI" "UA"]]])

(def User [:map [:id :string] [:address ]])

(-> {}
    (m/explain User)
    (me/humanize))
```

## Multi schemas (unions)
Similar to `{isOpen: false} | {isOpen: true, content: string}` in typescript

```clojure
(m/validate
  [:multi {:dispatch :type}
   [:sized [:map [:type :keyword] [:size :int]]]
   [:human [:map [:type :keyword] [:name :string] [:address [:map [:country :keyword]]]]]]
  {:type :sized, :size 10})
;; => true
```
