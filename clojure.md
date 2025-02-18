## Working around dynamic typing

The program should be designed in such a way that it is query-able in live runtime (repl).
Some techniques to achieve this are:

- Using the [Data as function](#data-as-function) idea.
- [Multimethod](#multimethod) feature instead of `if-else` or `cond` statements.
- Attaching [metadata](#metadata) to vars.

It is important to realize that Clojure does not have many data types.
Basically the main ones which are widely used are:

- collections (list, vector)
- hashmaps
- sets

Most of the stdlib is designed around these data types.
This results in a lot of functions which operate on few data types.
There is a famous quote which says
`It is better to have 100 functions operate on 10 data structures than 10 functions on 100 data structures.`
In contrast with a typed language, where we would have a constrained function which
for example only takes in a `User` object/record/struct.
So this `User` is basically a simple hashmap but we can't operate with
standard functions on it.

Also refer to the [naming conventions](#naming-conventions) section.

## Metadata

We can attach metadata to vars, functions, and data. This metadata can be used
for example to attach type hints, documentation, or to mark a var as private.

```clojure
;; Common documentation patterns:
(def ^{:doc "Stores user configuration"
       :example "{:name \"John\", :age 30}"
       :since "1.0.0"}
  user-config {})
```

Somehow the lsp can pickup those keywords and when using `K` for showing
the documentation, it will show the values.

Interesting is that when using

```clojure
(meta #'user-config)
```

And evaling it, it shows not only the stuff we attached but also the filename,
and the line of the function. Could be useful for logging

## Reader macro

A reader macro is a special character which says:
"From here on interpret the following sequence of characters in a special way."

Example:

```clojure
;; ' Quote - prevents evaluation
'(1 2 3)         ; => (1 2 3) as literal list, not a function call

;; # Dispatch macro - has multiple uses
#"hello"         ; => Regex pattern
#{1 2 3}         ; => Set literal
#(+ % 1)         ; => Anonymous function (shorthand)
#'symbol         ; => Var reference

;; ^ Metadata - attaches metadata to next form
^:private def x 1
^String name     ; => Type hint
```

## Data as function

Whenever possible it makes sense to write programs in such a way where
the data itself is a function. This is a powerful technique in Clojure.

[Detailed blog post](https://www.evalapply.org/posts/clojure-mars-rover/#rotations)

Example:

```clojure
(def rot-L {:N :W
            :S :E
            :W :S
            :E :N})

(comment
  (rot-L :N) ; => :W
  (rot-L (rot-L :N)) ; => :S
  )
```

## Multimethod

Is somewhat similar to polymorphism in OOP. It allows us to define a function
that behaves differently based on the input. The dispatch function is used to
determine which implementation to use.

```clojure

;; Define a multimethod with a dispatch function
(defmulti greet :type)

;; Define implementations for different cases
(defmethod greet :formal [person]
  (str "Hello, " (:name person) ", pleased to meet you."))

(defmethod greet :casual [person]
  (str "Hey " (:name person) "!"))

;; Usage:
(greet {:type :formal :name "Mr. Smith"})
;; => "Hello, Mr. Smith, pleased to meet you."
(greet {:type :casual :name "Bob"})
;; => "Hey Bob!"
```

The dispatch function doesn't have to be a keyword. In the example, `:type` is
a shorthand for `(fn [x] (:type x))`, but we can use any function as a dispatch
function.

## Repl

Start a repl session with

```bash
clj -M:repl/conjure
```

I use conjure for nvim. Then just open the editor in the same root directory.

### Repl tools

[Inline def debugging](https://blog.michielborkent.nl/inline-def-debugging.html)

With this technique we can debug by using def for function arguments.
It works but it is not very convenient.
A useful tool I found is [snitch](https://github.com/AbhinavOmprakash/snitch)
It lets you replace a defn with a defn* which stores all the arguments, lambdas
and let values in defs.
Clj-kondo (linter) will yell at defn* syntax so we have to let it know about it.

In `.clj-kondo/config.edn` (might need to create new if not present) add

```clojure
{:lint-as {snitch.core/defn* clojure.core/defn}
 :linters {:unresolved-symbol {:exclude [(snitch.core/defn*)]}}}
```

## Naming conventions

When reading clojure code it is important to know the naming conventions
developers use for naming things. Here are some common conventions:

```
m -> map
s -> string or set (context dependent)
xs -> sequence
v -> vector or value in a key/value context
k -> key
f -> function
coll -> collection
x, y, z -> a single element
e -> element or exception (context dependent)
idx -> index
s suffix -> a collection of items, like `users`, a collection of user
? suffix -> a boolean, like valid?
! suffix -> a function that you need to be careful for some reason, typically performs side-effect, or mutation
num -> a number
len -> length of something, a number as well
val -> some value
r, ret, result -> some result/return
```

## Testing

In `deps.edn` add kaocha (testing lib)

```
{ :deps ...
 :aliases {:test {:main-opts ["-m" "kaocha.runner"]
                  :extra-deps {lambdaisland/kaocha {:mvn/version "1.91.1392"}}}}}
```

Add `tests.edn` file

```
#kaocha/v1
 {:tests [{:id          :unit
           :test-paths  ["test" "src"]
           :ns-patterns [".*"]}]
  :watch? true}
```

Test file structure:

```clojure
(ns my-clojure-project.core-test
  (:require
   [clojure.test :as test]
   [my-clojure-project.core :as core]))

(test/deftest has-3-words-test
  (test/testing "false cases"
    (test/is (= false (core/has-3-words {:title "one two"})))
    (test/is (= false (core/has-3-words {:title "one two three four"}))))
  (test/testing "true case"
    (test/is (= true (core/has-3-words {:title "one two three"})))))

; For repl evaluation
(comment
    (test/run-tests 'my-clojure-project.core-test)
    (test/run-test has-3-words-test))
```

Run with

```
clj -M:test
```

## Using JVM interop

Import like

```clojure
(ns my-clojure-project.core
  (:import (java.time LocalDateTime)
           (java.time.format DateTimeFormatter)))
```

Consume like

```clojure
(LocalDateTime/parse "Jan 01 2021 00"
                     (DateTimeFormatter/ofPattern "MMM dd yyyy HH"))
```

## Making http requests

Add `clj-http/clj-http {:mvn/version "3.12.3"}` to `deps.edn`

Import like `[clj-http.client :as http]`

Consume like

```clojure
(->> (http/get "https://jsonplaceholder.typicode.com/todos" {:as :json})
     (:body))
```

## Parsing html dom

Add `hickory/hickory {:mvn/version "0.7.1"` to `deps.edn`

Import like `[hickory.select :as hs] [hickory.core :as hc]`

Consume like

```clojure
(defn get-muzon-dom []
    (->>
     (get-muzon-muzon-list)
     (hc/parse)
     (hc/as-hickory)
     (hs/select (hs/class "section"))))
```

## Live reloading when adding deps

Refer to [Official guide](https://clojure.org/reference/repl_and_main#add_lib)

Section `Adding libraries for interactive use`

Clojure 1.12 is required

Import tools like `[clojure.repl.deps :refer :all] `
Use and eval like `(clojure.repl.deps/sync-deps)`

## Sql queries

Add deps to `deps.edn`

```clojure
com.github.seancorfield/next.jdbc {:mvn/version "1.3.894"}
org.postgresql/postgresql {:mvn/version "42.6.0"}
com.github.seancorfield/honeysql {:mvn/version "2.6.1270"}
```

Postgresql is the driver for the database.
jdbc is the library for executing queries.
honeysql allows us to write sql queries as hashmaps and not as plain strings.

Import like

```clojure
   [honey.sql :as sql]
   [next.jdbc :as jdbc]
   [next.jdbc.result-set :as rs]
```

Query like

```clojure
(def db
  {:dbtype "postgresql"
   :dbname "signbnb"
   :host "localhost"
   :user "admin"
   :password "mysecretpassword"})

;; Create a connection pool (recommended for production)
(def datasource (jdbc/get-datasource db))

(defn simple-query []
  (jdbc/execute! datasource
                 (sql/format
                  {:select [:*],
                   :from :User :where [:= :id "6607dc80-6b0a-4be5-8dad-1715e60a7f0f"]},
                  {:quoted true})))
```

## Atoms

An atom is a thread-safe, mutable reference type in Clojure that provides a way
to manage shared, synchronous, independent state. Key points about atoms:

1. Basic usage:

```clojure
;; Create an atom
(def counter (atom 0))

;; Read value
@counter  ; => 0

;; Update value
(swap! counter inc)  ; => 1
```

2. Key characteristics:

- Thread-safe: Safe for concurrent access
- Synchronous: Updates happen immediately
- Independent: Not coordinated with other reference types

3. Common operations:

```clojure
(reset! counter 0)    ; Set direct value
(swap! counter + 5)   ; Update using function
(deref counter)       ; Another way to read value
```

## Async requests

In Clojure (and [Clojurescript](/clojurescript.md)),
core.async lib is used for handling asynchronous operations:

For Clojure, you would need to add it to your dependencies in `deps.edn`:

```clojure
org.clojure/core.async {:mvn/version "1.6.673"}
```

In Clojurescript, it appears to be already included in the Clojurescript
compiler.

The main difference is:

- In Clojure: `go` blocks are executed on a thread pool
- In ClojureScript: `go` blocks are implemented using JavaScript's event loop
  and callbacks

Both share the same API and concepts:

```clojure
(require '[clojure.core.async :refer [go chan <! >!]])

;; Works in both Clojure and ClojureScript
(let [c (chan)]
  (go
    (let [val (<! c)]
      (println val))))
```

The example you showed with HTTP requests might need different HTTP libraries:

- ClojureScript: typically uses `cljs-http`
- Clojure: typically uses `clj-http` or `http-kit`

The main concepts are:

1. `go`:

- Creates a go block (lightweight process/coroutine)
- Allows you to write asynchronous code in a synchronous style
- Returns a channel
- Code inside `go` blocks can use `<!` and `>!` operators

2. `<!`:

- Used to "park" (wait) and read a value from a channel
- Similar to `await` in other languages
- Can only be used inside `go` blocks
- Blocks execution until a value is available

3. A channel (`chan`) in core.async is a communication mechanism that allows
   values to be passed between different processes/threads/go blocks. Think of
   it as a queue where you can put values in and take values out.

Basic usage of channels:

```clojure
;; Create a channel
(def c (chan))

;; Put value into channel
(go (>! c 42))

;; Take value from channel using <!
(go (println (<! c)))  ; prints: 42
```

Channels are fundamental to core.async's CSP (Communicating Sequential
Processes) model of concurrency.

## Deps installation

1. Go to [https://clojars.org](https://clojars.org)
2. Search for the library you want to install
3. Copy the stuff from `Clojure CLI/deps.edn` section to deps.edn file

## Http server

In clojure world, there is `Ring` which is similar to http module in node.
`Ring` provides a simple and modular approach to handling HTTP requests and
responses. On top of `Ring`, the most used one appears to be [Compojure](#compojure) which
is a routing library similar to express in node.

## To read / to watch

[Interactive programming](https://www.youtube.com/watch?v=50vU6rp2jyA)

[Writing REPL-friendly programs](https://clojure.org/guides/repl/enhancing_your_repl_workflow#writing-repl-friendly-programs)

[Jettys threading architecture](https://jetty.org/docs/jetty/12/programming-guide/arch/threads.html)

[Var evaluation](https://srasu.org/posts/var-evaluation/)

[Exception handling](https://grishaev.me/clj-book-exceptions/)

## Specs

[How do you use clojure.spec](https://corfield.org/blog/2019/09/13/using-spec/)

## Tools suggestions to use

From a response in discord:

```
I mean, I'd use clojure.tools.logging not the java stdlib, or if I were writing an application and not a library I'd use taoensso/telemere
for db access yeah it's jdbc, but specifically I'd use next.jdbc with hikari for connection pooling
```

For db migrations people recommend [Flyway](https://www.red-gate.com/products/flyway/)

## Doto macro

The `doto` macro executes methods on an object in sequence and returns the
object itself. It's similar to method chaining in other languages.
I guess it is used when interoping with Java libraries.

`doto` is useful when you need to perform multiple operations on an object and
want to avoid verbose let bindings. Another example:

```clojure
;; Without doto
(let [sb (StringBuilder.)]
  (.append sb "Hello")
  (.append sb " ")
  (.append sb "World")
  sb)

;; With doto
(doto (StringBuilder.)
      (.append "Hello")
      (.append " ")
      (.append "World"))
```

## Java lsp integration

For lsp to consume docs and provide autocomplete for java static classes refer to
[Lsp docs](https://clojure-lsp.io/settings/#java-support)

Important part is documented here
```
Most JRE installations contains the java source code in a src.zip, clojure-lsp
tries to find it via :java :home-path setting if provided, JAVA_HOME env var or
java command on PATH, if found clojure-lsp extracts to its global cache dir
($XDG_CACHE or ~/.cache/clojure-lsp) to be used in other projects.
```

But I had openjdk 23 installed and it did not have a src.zip file.
I had to make sure I use openjdk 17 (it has a src.zip included)

Then in `clojure-project-root/.lsp/config.edn` add

```clojure
{:java {:home-path "/opt/homebrew/Cellar/openjdk@17/17.0.14/libexec/openjdk.jdk/Contents/Home"
        :decompile-jar-as-project? true
        :download-jdk-source? true}}
```

And since the src.zip is located in 
`/opt/homebrew/Cellar/openjdk@17/17.0.14/libexec/openjdk.jdk/Contents/Home/lib/src.zip`
I added a symlink to it one level up like
```bash
cd /opt/homebrew/Cellar/openjdk@17/17.0.14/libexec/openjdk.jdk/Contents/Home/
ln -s lib/src.zip src.zip
```

Also I reinstalled clojure lsp with mason and made sure that a `db.transit.json`
file appeared in `~/.cache/clojure-lsp/`
