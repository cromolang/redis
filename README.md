# Redis for Cromo

A client of [Redis](https://redis.io), and of the servers that speak its protocol, as Valkey, for a [Cromo](https://cromolang.org) program. Written in Cromo, on `std.net`: it brings no library with it.

```sh
cromo get github.com/cromolang/redis
```

```cromo
import redis

i32 main() {
    redis.Client store = redis.connect("localhost", 6379)
    store.set("greeting", "hello")
    string found
    if (store.get("greeting", ref found)) {
        println("{}", found)   // hello
    }
    println("{} visits", store.incr("visits"))
    ret 0
}
```

## What it has

- `redis.connect(host, port)` and `redis.connectTls(host, port)` give a `Client`, a connection to the server; `auth(password)` and `select(database)` when the server asks.
- `get(key, ref value)`, false when the key is not there, `set(key, value)`, `setFor(key, value, seconds)`, `remove(key)`, `exists(key)`, `expire(key, seconds)`, `keys(pattern)`.
- `incr(key)` and `incrBy(key, amount)`, for the counters.
- `ping()`.
- `command(words)` sends any command, its name and its arguments, and gives a `redis.Reply`: its `kind`, `Nil`, `Text`, `Integer` or `Array`, with its `text`, its `number` or its `items`. `store.command(["LPUSH", "queue", "job"])`.

What the server refuses is an `Error` with its message, as `Redis: ERR unknown command 'FLY'`.

## Limits of this version

- A client is for one thread at a time: each thread that talks to the server has its own.
- No publish and subscribe, no pipelines, no clusters: each command waits for its answer.
- The values are texts: bytes that are not UTF-8 are not read.
- The tests run against a small server in the test file, which speaks the protocol for a few commands: the package was not tried yet against a server of Redis.

## The versions

| This package | Cromo |
|---|---|
| v0.1.0 | 0.90 or later |

## Developing it

```sh
cromo test redis.cro
```

## License

MIT: see `LICENSE`.
