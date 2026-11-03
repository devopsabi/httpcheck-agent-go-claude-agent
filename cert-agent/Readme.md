☁  cert-agent  go run . -config checks.yaml
[tool] list_targets {}
[tool] run_cert_checks {"hosts":["www.google.com","devopsabi.de","devopsabhi.com"]}
# Certificate Expiry Report

## Summary
Two of the three configured targets are critical. The production host `devopsabi.de` has a Let's Encrypt certificate with 14 days left. `devopsabhi.com` could not be checked because its DNS name does not resolve. `www.google.com` is healthy. No hosts were skipped.

**Critical: 2 / Warning: 0 / OK: 1 / Skipped: 0**

## Findings

### [CRITICAL] devopsabi.de
- **Check**: prod-public (warn 45 days, critical 21 days)
- **Problem**: The certificate expires in 14 days, which is below the critical threshold of 21. It is a production public host. Once the certificate expires, customers will get browser errors. The issuer is Let's Encrypt (CN=YE2), and these certificates should renew automatically at 30 days left. At 14 days, auto-renewal (cert-manager or similar) is probably broken. The chain is valid and the host uses TLS 1.3. The certificate has a lifetime of 89 days and was issued on 2026-08-20.
- **Days left**: 14 (expires 2026-11-18)
- **Next step**: Renew the certificate now, either manually or by forcing a cert-manager renewal. Then find out why automatic renewal did not run. Check the cert-manager logs, the state of the Certificate and Order resources, and whether the ACME challenge can succeed (DNS or HTTP-01).

### [CRITICAL] devopsabhi.com
- **Check**: defaults (warn 30 days, critical 14 days)
- **Problem**: The connection failed with `dial tcp: lookup devopsabhi.com: no such host`. The DNS name does not resolve, so no certificate could be inspected. Expiry, chain and policy status are unknown.
- **Days left**: unknown (no certificate retrieved)
- **Next step**: Check that the domain is registered and not expired, and that its DNS records exist and resolve. If the host was decommissioned, remove it from the target list. If the name is a typo, correct it. For example, `devopsabi.de` may have been intended. Then re-run the check.

## Patterns
- Both critical findings are availability problems for a public name. Their causes differ: one is an expiring certificate and the other is a DNS resolution failure. The tool results do not show a shared cause.
- The `devopsabi.de` Let's Encrypt certificate is inside the 30-day renewal window. Check any other cert-manager-managed certificates for the same renewal problem.

## Healthy hosts
- www.google.com: 56 days left (expires 2026-12-30)

STATUS: CRITICAL
exit status 2
