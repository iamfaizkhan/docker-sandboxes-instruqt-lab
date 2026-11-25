---
slug: launch-your-first-sandbox
id: qubvejdqcjt6
type: challenge
title: Launch your first sandbox
teaser: Start an isolated microVM with one command and find it from the admin's side.
notes:
- type: text
  contents: |
    Your lab machine is starting. It has the Docker Sandboxes CLI (sbx) installed and signed in for you.
tabs:
- id: i5zmcgdxjwyn
  title: Sandbox
  type: terminal
  hostname: lab-machine
- id: 8katbu3k9hhc
  title: Admin
  type: terminal
  hostname: lab-machine
difficulty: basic
timelimit: 600
enhanced_loading: null
---
You'll use two terminals in this lab:

- **Sandbox**: where you work *inside* the sandbox, seeing what the agent sees.
- **Admin**: where you stay *outside*, on the lab machine, seeing what an IT admin sees.

## 1. Launch the sandbox

In the **Sandbox** tab, run:

```bash
sbx run --name my-sandbox shell .
```

| Part | Meaning |
|---|---|
| `run` | Create a sandbox and connect to it |
| `--name my-sandbox` | Give it a predictable name |
| `shell` | Use a plain terminal instead of an AI agent, so no AI subscription is needed |
| `.` | Share only the current folder (`project`) with the sandbox |

> [!IMPORTANT]
> The `.` shares the folder you are standing in. Always launch from a dedicated project folder, never from your home folder.

The first launch takes a minute or two. When your prompt changes to `agent@my-sandbox`, you're inside.

## 2. Find it from the admin's side

In the **Admin** tab, run:

```bash
sbx ls
```

You should see `my-sandbox` with the status `running`.

> [!TIP]
> If your Sandbox tab's prompt changes back to `#`, you've been disconnected and are back on the lab machine. Reconnect with `sbx run --name my-sandbox`.

> [!NOTE]
> Each new challenge opens fresh terminals on the lab machine. Your sandbox keeps its files and containers, and you'll reconnect to it with `sbx run --name my-sandbox`.

When you see `my-sandbox` running, click **Check**.