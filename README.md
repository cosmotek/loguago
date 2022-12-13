# loguago
Zerolog wrapper for Gopher-Lua | [godoc](https://godoc.org/github.com/rucuriousyet/loguago)

Contributions welcome!

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

	// create a standard zerolog instance with your desired config (just an example)
	zlogger := zerolog.New(os.Stdout).With().Str("unit", "my-lua-module").Logger()
	
	// wrap the zerolog instance as a loguago logger
	logger := loguago.NewLogger(zlogger)

	// load the loguago logger as a Gopher-Lua module with the module name "logger" (used for require)
	state.PreloadModule("logger", logger.Loader)
  
  	// the rest of your program lives here
  	// ...
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

-- each logging function takes a message and a table with an indeterminate
-- number of fields to provide context within the log messages. For example, (depending
-- on how you configure the zerolog instance) this:
logger.info("foo bar!", {name="seth", age=1000, engineer=true})
-- should produce something like: "1494567715 |INFO| foo bar! name=seth age=1000 engineer=true"
```
