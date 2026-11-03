☁  cert-agent  go run . -config checks.yaml
[tool] list_targets {}
[tool] run_cert_checks {"hosts":["www.google.com","devopsabi.de","devopsabi.com"]}
# Certificate Expiry Report

## Summary
Both production customer-facing hosts, devopsabi.de and devopsabi.com, are critical. Each has 14 days left, which is under the 21-day critical threshold of the prod-public check. Both certificates come from Let's Encrypt and were issued within about a minute of each other. Auto-renewal (cert-manager) is therefore probably broken, because these certificates should have renewed at 30 days left. www.google.com is healthy. All three configured targets were checked, and none were skipped.

**Critical: 2 / Warning: 0 / OK: 1 / Skipped: 0**

## Findings

### [CRITICAL] devopsabi.de
- **Check**: prod-public (warn 45 days, critical 21 days)
- **Problem**: The certificate expires in 14 days. It is a Let's Encrypt certificate (issuer CN=YE2), which should have renewed at 30 days left. Auto-renewal (cert-manager) is probably broken. If it expires, customers will see browser errors. The chain is valid, TLS 1.3 is in use, and there are no other findings.
- **Days left**: 14 (expires 2026-11-18)
- **Next step**: Check cert-manager (or the ACME client in use). Look at the Certificate, Order and Challenge resources, the logs, and whether the ACME challenge is failing. Fix the cause and force a renewal. Then confirm the new expiry date.

### [CRITICAL] devopsabi.com
- **Check**: prod-public (warn 45 days, critical 21 days)
- **Problem**: The certificate expires in 14 days. It is a Let's Encrypt certificate (issuer CN=YE1), which should have renewed at 30 days left. Auto-renewal (cert-manager) is probably broken. If it expires, customers will see browser errors. The chain is valid, TLS 1.3 is in use, and there are no other findings.
- **Days left**: 14 (expires 2026-11-18)
- **Next step**: Handle this together with devopsabi.de. Fix the renewal pipeline, force a renewal, and verify the new certificate is served.

## Patterns
- **Same expiry date**: devopsabi.de and devopsabi.com were issued on 2026-08-20 and both expire on 2026-11-18. They were issued about a minute apart, so one shared renewal job or cert-manager setup probably handles both. That job is probably the single point of failure. I can't tell the cause from these results.
- **Renewal window missed**: Both certificates are already past the 30-day renewal point. This supports the theory that renewal has been failing for about two weeks.

## Healthy hosts
- www.google.com: 56 days left

STATUS: CRITICAL
exit status 2
