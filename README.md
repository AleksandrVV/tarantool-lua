lua-tarantool-client
===================

Driver for tarantool 1.7+ on plain lua sockets

Introduction
------------

A pure Lua driver for the NoSQL database [Tarantool](http://tarantool.org/) using [luasocket](https://github.com/diegonehab/luasocket).

Requires [lua-MessagePack](https://github.com/fperrad/lua-MessagePack).

luasocket
-------

For `luasocket` sockets and [sha1.lua](https://github.com/kikito/sha1.lua) are required.

These can be installed using `luarocks install lua-resty-socket` and `luarocks install sha1`


Synopsis
------------

```lua

tarantool = require("tarantool")

-- initialize connection
local tar = tarantool({
    host           = '127.0.0.1',
    port           = 3301,
    user           = 'gg_tester',
    password       = 'pass',
    socket_timeout = 2000,
    connect_now    = true,
})

-- requests
local data, err = tar:ping()
local data, err = tar:insert('profiles', { 1, "nick 1" })
local data, err = tar:insert('profiles', { 2, "nick 2" })
local data, err = tar:select(2, 0, 3)
local data, err = tar:select('profiles', 'uid', 3)
local data, err = tar:replace('profiles', {3, "nick 33"})
local data, err = tar:delete('profiles', 3)
local data, err = tar:update('profiles', 'uid', 3, {{ '=', 1, 'nick new' }})
local data, err = tar:update('profiles', 'uid', 3, {{ '#', 1, 1 }})

-- disconnect or set_keepalive at the end
local ok, err = tar:disconnect()
local ok, err = tar:set_keepalive()

```

Hacking
-------

Module contains implementations written in Lua and
[Moonscript][moonscript-url]. First one could be generated using second one
using the Moonscript compiler:

```sh
$ luarocks --local install moonscript
$ export PATH=$PATH:$(luarocks path --lr-bin)
$ moonc -o tarantool.lua tarantool.moon
```

[moonscript-url]: https://moonscript.org/
