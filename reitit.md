# Reitit

## Course notes

### [09 - Start](https://www.jacekschae.com/view/courses/learn-reitit-pro/470079-server/1350402-09-start)

- Instead of calling the request I can just take the app and call it with a
  hash map like [

```clojure
(app {:request-method :get :uri "/"})
```

### [23 - snake-kebab](https://www.jacekschae.com/view/courses/learn-reitit-pro/507643-recipes/1591082-23-snake-kebab)

- Postgres table keys are usually snake_case but in clojure we use kebab-case
  so we need to convert them. In the video it is explained how we can
  configure jdbc globally with integrant such that it converts the keys for us.

### [24 - Update Recipe](https://www.jacekschae.com/view/courses/learn-reitit-pro/507643-recipes/1591149-24-update-recipe)

```clojure
(set! *print-namespace-maps* <false|true>)
```

- This will print the map without the namespace. This might be easier to read in repl
  but it is not affecting the actual data.

### [34 - Developer experience](https://www.jacekschae.com/view/courses/learn-reitit-pro/583403-favorite-unfavorite-recipe/1677159-34-developer-experience)

- Reitit provides a nice helper for pretty printing exceptions for debugging.
  In router config we can add

```clojure
{:exception reitit.dev.pretty/exception}
```

- Also there is a validator which checks that handlers exist and are valid functions
```clojure
{:validate reitit.ring.spec/validate}
```
