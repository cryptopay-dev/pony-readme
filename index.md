# Welcome to Pony
Pony is a small Go library that helps you rapidly create projects.

## Examples
You can look in [/example](github.com/cryptopay-dev/pony-readme) folder to find out more examples.

### Minimal example
```go
package main

import (
	"context"
	"net/http"
	
	"github.com/cryptopay-dev/pony"
	"github.com/cryptopay-dev/pony/digger"
	"github.com/cryptopay-dev/pony/servers"
	
	"go.uber.org/zap"
)

func newApp() http.Handler {
	return http.HandlerFunc(func(w http.ResponseWriter, r *http.Request) {
		w.Write([]byte(`Hello, world!`))
	})
}

func main() {
	p, err := pony.New(pony.Params{
		Name:               "minimal",
		LoadDefaultModules: true,
	}, digger.Module{
		{CreateFunc: newApp},
	})
	pony.FailOnError(err)
	pony.FailOnError(p.RunFunc(func(log *zap.SugaredLogger, srv *servers.Container, ctx context.Context) error {
		log.Info("Stargin servers")
		srv.Servers().Start()

		<-ctx.Done()

		log.Info("Stopping servers")
		srv.Servers().Stop()

		return nil
	}))
}
```

## Modules
Pony have a list of useful modules based on:
- Configuration **Viper**
- Logging **zap**
- PostgreSQL **go-pg**
- Sentry **raven**
- Web **echo**
- Workers **chapsuk/worker**

### Configuration
### PostgreSQL
### Sentry
### Workers
### Web server

## Cookbook
### Twirp Service
