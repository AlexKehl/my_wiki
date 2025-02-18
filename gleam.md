## Doing http requests and decoding JSON

```gleam
import gleam/dynamic/decode
import gleam/http/request
import gleam/httpc
import gleam/json
import gleam/result

pub type Err {
  JsonDecode(json.DecodeError)
  Http(httpc.HttpError)
}

pub type Todo {
  Todo(user_id: Int, id: Int, title: String, completed: Bool)
}

fn todo_decoder() -> decode.Decoder(Todo) {
  use user_id <- decode.field("userId", decode.int)
  use id <- decode.field("id", decode.int)
  use title <- decode.field("title", decode.string)
  use completed <- decode.field("completed", decode.bool)
  decode.success(Todo(user_id:, id:, title:, completed:))
}

pub type Todos {
  Todos(todos: List(Todo))
}

fn fetch_todos() {
  let req =
    request.new()
    |> request.set_host("jsonplaceholder.typicode.com")
    |> request.set_path("/todos/")
    |> httpc.send()
    |> result.map_error(Http)

  use resp <- result.try(req)

  Ok(resp.body)
}

fn parse_todos(todo_string: String) {
  todo_string
  |> json.parse(decode.list(todo_decoder()))
  |> result.map_error(JsonDecode)
}

pub fn main() {
  use todos <- result.try(fetch_todos())
  use parsed_todos <- result.try(parse_todos(todos))
}
```
