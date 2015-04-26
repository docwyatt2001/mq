# MQ

A simplistic pub/sub message queue for testing. Purely a toy project. Discovery, clustering, etc coming soon.

Subscription uses websockets.

## Pub

```
curl -d "A completely arbitrary message" "http://localhost:8081/pub?topic=foo"
```

## Sub

```
curl -i -N -H "Connection: Upgrade" -H "Upgrade: websocket" -H "Host: localhost:8081" -H "Origin:http://localhost:8081" "http://localhost:8081/sub?topic=foo"
```

## Pub using Go Client

```go
package main

import (
	"log"
	"time"

	"github.com/asim/mq/go/client"
)

func main() {
	tick := time.NewTicker(time.Second)

	for _ = range tick.C {
		if err := client.Publish("foo", []byte(`bar`)); err != nil {
			log.Println(err)
			break
		}
	}
}
```

## Sub using Go Client

```go
package main

import (
	"log"

	"github.com/asim/mq/go/client"
)

func main() {
	ch, err := client.Subscribe("foo")
	if err != nil {
		log.Println(err)
		return
	}

	for e := range ch {
		log.Println(string(e))
	}

	log.Println("channel closed")
}
```