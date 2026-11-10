# Agenten bei der Arbeit

Beispiel:

```goat
☁  cert-agent  go run . -config checks.yaml                                                                                                         
[tool] list_targets {}
[tool] run_cert_checks {"hosts":["www.google.com","devopsabi.de","devopsabi.com"]}
# Certificate Expiry Report

## Summary
I checked all 3 configured targets. All of them returned OK. Each has a valid chain and uses TLS 1.3. No host is near its expiry threshold, and none was skipped.

- **Critical**: 0
- **Warning**: 0
- **OK**: 3
- **Skipped**: 0

## Findings
No hosts have findings.

## Patterns
- `devopsabi.de` and `devopsabi.com` are both Let's Encrypt (YE1) certificates. They were issued about a minute apart on 2026-11-10 and expire on the same date, 2027-02-08. This looks like one shared renewal job, probably cert-manager. Both are at 89 days left, so renewal is working now. If that job fails later, both hosts will expire together.
- Let's Encrypt renewal should happen at about 30 days left. Watch these two hosts if they fall below that.

## Healthy hosts
Production customer-facing hosts (`prod-public`) are listed first.

| Host | Check | Days left | Expires |
|---|---|---|---|
| devopsabi.de | prod-public | 89 | 2027-02-08 |
| devopsabi.com | prod-public | 89 | 2027-02-08 |
| www.google.com | defaults | 50 | 2026-12-30 |

STATUS: OK

```
