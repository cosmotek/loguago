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

	state.PreloadModule("logger", logger.Loader)
  
  // the rest of your program lives here
  ...
}
```

```lua
local logger = require("logger")

-- the following functions all have the same signature but different names to
-- allow for log levelling. `logger.<level>(message, fields)`
logger.info("msg", {name="seth", isCoder=true})
logger.debug
logger.warn
logger.error

-- basically each logging function takes a message and a table with an indeterminate
-- number of fields to provide context within the log messages. For example, (depending
-- on how you configure the zerolog instance) this:
logger.info("I like fruit!", {name="seth", age=1000, male=true, engineer=true})
-- should produce something like: "1494567715 |INFO| I like fruit! name=seth age=1000 male=true engineer=true"
```
