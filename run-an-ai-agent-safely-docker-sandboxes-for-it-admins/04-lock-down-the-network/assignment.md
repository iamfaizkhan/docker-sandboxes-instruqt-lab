---
slug: lock-down-the-network
id: jq17yiqvazqk
type: challenge
title: Lock down the network
teaser: Switch to the Locked Down policy, watch a request get blocked, then allow
  exactly one destination.
notes:
- type: text
  contents: |
    Every request a sandbox makes passes through a checkpoint on the lab machine. Now you'll set its rules.
tabs:
- id: bkvb2gnyadse
  title: Sandbox
  type: terminal
  hostname: lab-machine
- id: mlklsjjnib3m
  title: Admin
  type: terminal
  hostname: lab-machine
difficulty: intermediate
timelimit: 900
enhanced_loading: null
---
Every request from a sandbox goes through a checkpoint on the lab machine, and the checkpoint follows a **network policy**. Your lab machine uses the **Balanced** preset: *default deny, with common dev sites allowed*.

## 1. See the current rules

In the **Admin** tab, run:

```bash
sbx policy ls
```

`local-policy` shows about **195** network allow rules. Anything not on that list is blocked by `default-deny-all`.

## 2. Switch to Locked Down

> [!WARNING]
> Changing the preset restarts the sandbox service and **stops running sandboxes**. In production, set the policy *before* rolling sandboxes out, not while people are working.

In the **Admin** tab, run:

```bash
sbx policy reset
```

1. Type `y` to confirm.
2. When asked to choose a policy, choose **3 — Locked Down** (*all network traffic blocked unless you allow it*).

Run `sbx policy ls` again. The network allow rules are gone.

## 3. Reconnect, and watch a request get blocked

Your Sandbox tab was disconnected. In the **Sandbox** tab, reconnect:

```bash
sbx run --name my-sandbox
```

Then try to reach GitHub, as an agent might when downloading code:

```bash
curl -I https://github.com
```

You'll see `403 Forbidden`. The checkpoint answered on GitHub's behalf. GitHub never saw the request.

## 4. Read the audit log

In the **Admin** tab, run:

```bash
sbx policy log
```

Under **BLOCKED REQUESTS**, you'll see `github.com:443` with the reason `No matching allow rule (default deny)`. This is how admins find out what their agents are trying to reach.

## 5. Allow exactly one destination

In the **Admin** tab, run:

```bash
sbx policy allow network github.com
```

Rules take effect immediately. Nothing restarts.

## 6. Try again

In the **Sandbox** tab, run:

```bash
curl -I https://github.com
```

This time you'll see `HTTP/2 200`, a real answer from GitHub.

> [!IMPORTANT]
> `github.com` allows only that exact address. Code downloads often come from other addresses, such as `raw.githubusercontent.com`, which are still blocked. A wildcard like `**.github.com` covers addresses *under* github.com, but not `github.com` itself.

When `github.com` is allowed and the log shows your earlier blocked request, click **Check**.