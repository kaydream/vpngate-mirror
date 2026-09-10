# VPN Gate server-list mirror

A tiny automatic mirror of the public [VPN Gate](https://www.vpngate.net/en/)
server list, for use in networks where `www.vpngate.net` is unreachable
(some carriers DNS-hijack or block the domain).

A GitHub Action fetches the official CSV endpoint
`http://www.vpngate.net/api/iphone/` every 6 hours from GitHub's runners
(which are not censored), sanity-checks it, and commits it as
[`servers.csv`](servers.csv).

## Use it

```
https://raw.githubusercontent.com/kaydream/vpngate-mirror/main/servers.csv
```

Add that URL as a "server list mirror" in an app that supports
user-configurable HTTPS mirrors (e.g. SoftEtherForMobile:
Settings → Server list → mirrors), and the server list will load even where
the official domain is blocked.

## Integrity

- The workflow commits the official response **byte-for-byte**, after
  checking that it starts with the `*HostName,…` header and ends with
  `#END`. Anything else is discarded and never committed.
- Raw.githubusercontent serves over HTTPS, so the list cannot be silently
  rewritten in transit by a carrier.
- The mirror is as trustworthy as this GitHub account. If you are not
  kaydream, treat any third-party mirror with appropriate suspicion — a
  poisoned server list points your VPN at someone else's servers.

## Data freshness

VPN Gate servers churn within hours; this mirror refreshes every 6 hours
(plus manual runs via workflow dispatch). Server entries may already be
offline by the time you connect — that is inherent to VPN Gate, not to the
mirror.
