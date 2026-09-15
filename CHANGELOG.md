# Changelog

## 0.1.0

- Initial HTTP/HTTPS client: `HTTPClient` (`FromConfig`/`Start`/`Stop`/
  `Send`) built on `net/http`. GET/POST/PUT/DELETE, standard headers,
  multipart bodies, blocked-status-code detection (`BlockReport`),
  optional `ProxyURL` (http/https/socks5).
- Depends on `ra-common-go` for `messaging.Envelope`.
- Client (outbound `sendOut`) only — no local server/SPA/WebSocket hosting
  (see `DESIGN.md`).
