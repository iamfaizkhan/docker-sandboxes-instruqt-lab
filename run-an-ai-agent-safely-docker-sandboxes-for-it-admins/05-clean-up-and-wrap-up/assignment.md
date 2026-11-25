---
slug: clean-up-and-wrap-up
id: jws9vdm7qyyj
type: challenge
title: Clean up and wrap up
teaser: Remove the sandbox, confirm your project files are safe, and review what you've
  learned.
notes:
- type: text
  contents: |
    Unused sandboxes take up resources and can hold stale data. Good admins clean up.
tabs:
- id: uodvlc59gkpy
  title: Admin
  type: terminal
  hostname: lab-machine
difficulty: basic
timelimit: 600
enhanced_loading: null
---
Unused sandboxes take up disk space and can hold stale code or data. In this last challenge you'll check your sandbox's status and then remove it completely.

## 1. Check the status

In the **Admin** tab, run:

```bash
sbx ls
```

`my-sandbox` is probably `stopped`. When nothing is connected to a sandbox, `sbx` switches it off to save resources, but keeps its files and containers. You could start it again with `sbx run --name my-sandbox`.

If it shows `running`, switch it off yourself:

```bash
sbx stop my-sandbox
```

## 2. Remove it

```bash
sbx rm my-sandbox
```

Type `y` to confirm. The default answer is **No**, so pressing Enter on its own deletes nothing.

Run `sbx ls` again. You'll see `No sandboxes found`.

## 3. Confirm your project is safe

```bash
ls -d /root/project
```

The project folder is still there. Removing a sandbox deletes the sandbox and everything inside it, including `agent-app`, but never the folder you shared with it.

## What you've learned

| You did | Why it matters to an admin |
|---|---|
| Ran an agent in its own microVM | Its own kernel, filesystem and Docker Engine, so a mistake stays inside |
| Proved the walls work both ways | The agent can't see host files or containers, and the host can't see the agent's |
| Switched to Locked Down and allowed one domain | Least privilege for network access, with a clear audit trail |
| Removed the sandbox | No stale environments left behind |

> [!NOTE]
> You set these rules on one machine. With **Docker AI Governance**, an organization sets them centrally for every developer, and local rules can't override them.

When `my-sandbox` is removed, click **Check** to finish the lab.