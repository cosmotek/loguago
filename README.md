# loguago
Zerolog wrapper for Gopher-Lua

[godoc](https://godoc.org/github.com/rucuriousyet/loguago)

Example usage:
```go
package main

import (
	"fmt"
	"io/ioutil"
	"os"

	"github.com/rs/zerolog"
	lua "github.com/yuin/gopher-lua"
	"github.com/rucuriousyet/loguago"
)

func main() {
	state := lua.NewState()
	defer state.Close()

	zlogger := zerolog.New(os.Stdout)
	logger := loguago.NewLogger(zlogger.With().Str("unit", "my-lua-module").Logger())

	state.PreloadModule("stackd.logger", logger.Loader)
  
  // the rest of your program lives here
  ...
}
