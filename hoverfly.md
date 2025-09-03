# Hoverfly

Is a mock server which I utilize for e2e testing.
Instead of mocking the api in the application itself I just change the
path in the env of the application for test case.

## Installation

Is installed with docker

```bash
docker run -d -p 8888:8888 -p 8500:8500 spectolabs/hoverfly:latest -webserver
```

Or add to `docker-compose.yml`

```yaml
  hoverfly:
    image: spectolabs/hoverfly:latest
    command: -webserver
    ports:
      - "8888:8888"
      - "8500:8500"
    restart: always
```

## Main commands

In test utils directory create a hoverfly directory with contents like.

```bash
.
├── error-simulation.json
└── success-simulation.json
```

`success-simulation.json`

```json
{
  "data": {
    "pairs": [
      {
        "request": {
          "path": [{ "matcher": "exact", "value": "/api/pokemon" }],
          "method": [{ "matcher": "exact", "value": "GET" }]
        },
        "response": {
          "status": 200,
          "body": "{\"name\":\"pikachu\"}",
          "headers": {
            "Content-Type": ["application/json; charset=utf-8"]
          }
        }
      }
    ]
  },
  "meta": {
    "schemaVersion": "v5",
    "hoverflyVersion": "v1.0.0",
    "timeExported": "2019-05-30T22:14:24+01:00"
  }
}
```

`error-simulation.json`

```json
{
  "data": {
    "pairs": [
      {
        "request": {
          "path": [{ "matcher": "exact", "value": "/api/pokemon" }],
          "method": [{ "matcher": "exact", "value": "GET" }]
        },
        "response": {
          "status": 400,
          "body": "{\"success\": false}",
          "headers": {
            "Content-Type": ["application/json; charset=utf-8"]
          }
        }
      }
    ]
  },
  "meta": {
    "schemaVersion": "v5",
    "hoverflyVersion": "v1.0.0",
    "timeExported": "2019-05-30T22:14:24+01:00"
  }
}
```

Then a simulation can be activated with 

```bash
curl -X PUT \
  -H "Content-Type: application/json" \
  --data @success-simulation.json \
  http://localhost:8888/api/v2/simulation
```

Keep in mind that PUT overwrites the current simulation set completly.
Instead, to append a simulation use POST.

Then the simulation can be queried with

```bash
curl http://localhost:8500/api/pokemon
```

## Other commands

Query simulations

```bash
curl -s 'http://localhost:8888/api/v2/simulation'
```

Delete simulations

```bash
curl -s -X DELETE 'http://localhost:8888/api/v2/simulation'
```
