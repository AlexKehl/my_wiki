## Reagent

Reagent is a thin layer on top of react which allows to write in clojurescript.
To get started I used this template

```bash
lein new reagent-frontend myproject
```

This are according to the documentation
`the assets for ClojureScript without a Clojure backend`

For some reason the other template I could not get to work with live reload and stuff.

It appears deps are not in `deps.edn` like in clojure projects but in `shadow-cljs.edn`

## State management

For state management reagent uses [atoms](/clojure.md#atoms).
Every time the atom changes components which use it are re-rendered.

## Http requests

Add `[cljs-http "0.1.46"]` to `shadow-cljs.edn`
Import like `[cljs-http.client :as http]`

And consume like

```clojure
(defn fetch-todos []
  (go
    (let [response (<! (http/get "https://jsonplaceholder.typicode.com/todos"))]
      (reset! todos (:body response)))))
```

## Storybook stuff

Before it was devcards but seems that it is not maintained anymore.

One alternative is
[Workspaces](https://github.com/nubank/workspaces)

Or 
[Portfolio](https://github.com/cjohansen/portfolio)

## Interesting libs

-[uix](https://github.com/pitch-io/uix) - Basically a react wrapper
