# Agenten bei der Arbeit

Beispiel:

```goat
☁  httpcheck-agent  go run . "Check https://google.com"                                                                                                  
[tool call] check_http_status {"urls":["https://google.com"]}
| URL | Status | Latency |
|---|---|---|
| https://google.com | 200 OK | 675 ms |

**Problems**
- None serious. The request redirected from `https://google.com` to `https://www.google.com/`. This is the normal canonical redirect to the `www` host, so it isn't a fault.
- Latency was 675 ms, under the 1000 ms threshold. It's acceptable, though not especially fast for Google. That figure probably includes the redirect hop and the network path from the checker.

**Next step:** None needed. If you want to skip the redirect in future checks, use `https://www.google.com/` directly.
☁  httpcheck-agent  
```
