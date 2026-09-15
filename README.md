# http-client (Go)

A direct (non-anonymized) HTTP/HTTPS client for **1M5**: builds a request
from a `messaging.Envelope` (URL, action, headers, body) and writes the
response back onto it.

A Go port of the client (outbound `sendOut`) side of
[`http-client-java`](https://github.com/resolvingarchitecture/http-client-java)'s
`ra.http.HTTPService`. The Jetty-based local server / SPA / WebSocket
hosting side of `HTTPService` is not ported — no other language port has
needed it yet; see `DESIGN.md`.

## Use

```go
import (
    "github.com/resolvingarchitecture/ra-common-go/messaging"
    httpclient "github.com/resolvingarchitecture/http-client-go"
)

client := httpclient.NewHTTPClient()   // or httpclient.FromConfig(cfg)

env := messaging.DocumentEnvelope()    // must be a document envelope - AddContent needs it
url := "https://resolvingarchitecture.io"
env.URL = &url
action := messaging.ActionGet
env.ActionValue = &action

if client.Send(env) {                  // false (cleanly) on any failure
    body := env.Content().([]byte)     // response body
} else {
    _ = env.ErrorMessages()            // what went wrong
}
```

`Start()`/`Stop()` are optional — `Send` lazily connects on first use, same
as `ra.http.HTTPService#sendOut`.

### Config keys

| key | default | meaning |
|-----|---------|---------|
| `ra.http.client.trustallcerts` | `false` | skip TLS certificate verification (test-only) |
| `ra.http.client.requestTimeoutSecs` | `60` | per-request timeout |
| `ra.http.client.proxyURL` | unset | route requests through this proxy — `http://`, `https://`, or `socks5://` |

## Build

```
go build ./...
go test ./...
go vet ./...
gofmt -l .
```

Depends on `ra-common-go` via a `go.mod` `replace` directive pointing at
`../../common/ra-common-go`, matching every other Go port's
monorepo-dependency convention.

## Status

HTTP and HTTPS GET/POST/PUT/DELETE work, including multipart bodies and the
standard header set (`Authorization`, `Content-Type`,
`Content-Disposition`, `Content-Transfer-Encoding`, `User-Agent`). No
redirect following beyond `net/http`'s default, no connection pooling
tuning, no local server/SPA/WebSocket hosting. See `DESIGN.md` and
`TODO.md`.
