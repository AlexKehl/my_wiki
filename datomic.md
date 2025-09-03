# Datomic

## Setup

```clojure
(:require [datomic.api :as d])

(def db-uri "datomic:dev://localhost:4334/hello")

(d/create-database db-uri)
```

Get connection like

```clojure
(def conn (d/connect db-uri))
```

And get snapshot like

```clojure
(def db (d/db conn))
```

## Queries
Simple:

```clojure
(d/q '[:find ?title :where
         [?e :movie/release-year 1985]
         [?e :movie/title ?title]]
       db)
```

This returns only the title. Something similar to `SELECT *` is `pull`. Like

```clojure
(d/q '[:find (pull ?e [*])
       :where [?e :movie/release-year 1985]]
     db)
```

## Schema

```clojure
(def movie-schema
  [{:db/ident       :movie/title
    :db/valueType   :db.type/string
    :db/cardinality :db.cardinality/one
    :db/doc         "The title of the movie"}

   {:db/ident       :movie/genre
    :db/valueTyp    :db.type/string
    :db/cardinality :db.cardinality/one
    :db/doc         "The genre of the movie"}

   {:db/ident       :movie/release-year
    :db/valueType   :db.type/long
    :db/cardinality :db.cardinality/one
    :db/doc         "The year the movie was released in theaters"}])
```
