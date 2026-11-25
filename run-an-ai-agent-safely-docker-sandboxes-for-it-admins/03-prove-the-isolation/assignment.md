---
slug: prove-the-isolation
id: qpoyym4am668
type: challenge
title: Prove the isolation
teaser: Show that the agent can't see the host's files or containers, and that the
  host can't see the agent's.
notes:
- type: text
  contents: |
    Your lab machine now holds a confidential folder and a running container. Let's see whether the sandbox can reach them.
tabs:
- id: mhv5g1ji027e
  title: Sandbox
  type: terminal
  hostname: lab-machine
- id: lhewagk98li5
  title: Admin
  type: terminal
  hostname: lab-machine
difficulty: basic
timelimit: 600
enhanced_loading: null
---
Your lab machine now holds two things an agent should never touch: a folder of customer data exports, and a running container. You'll look at each one from both sides of the wall.

## 0. Reconnect to your sandbox

New challenge, new terminals. In the **Sandbox** tab, run:

```bash
sbx run --name my-sandbox
```

Wait for the `agent@my-sandbox` prompt. If it ever changes back to the lab machine's prompt, run this again.

## 1. Files: the admin's view

In the **Admin** tab, run:

```bash
ls /root/customer-exports
```

You'll see a file of customer records. It's on the lab machine, one folder away from the project.

## 2. Files: the agent's view

In the **Sandbox** tab, run the same command:

```bash
ls /root/customer-exports
```

You'll get `No such file or directory`. The sandbox shares only `/root/project`. To the agent, nothing else on the lab machine exists.

## 3. Containers: the admin's view

In the **Admin** tab, run:

```bash
docker ps
```

You'll see `billing-app`, a container running on the lab machine.

## 4. Containers: the agent's view

In the **Sandbox** tab, run:

```bash
docker ps
```

The list is empty. The sandbox has its **own** Docker Engine, with no path to the lab machine's.

## 5. Run a container as the agent

Still in the **Sandbox** tab, start a container, as an agent might while it works:

```bash
docker run -d --name agent-app alpine sleep 3600
```

| Part | Meaning |
|---|---|
| `-d` | Run in the background |
| `--name agent-app` | Name the container |
| `alpine` | A tiny Linux image, downloaded from Docker Hub |
| `sleep 3600` | Keep it running for an hour |

Run `docker ps` again in the **Sandbox** tab. You'll see `agent-app`.

## 6. Check from the admin's side

In the **Admin** tab, run `docker ps` again. You'll still see only `billing-app`. The wall works in both directions.

> [!NOTE]
> Why it matters: the lab machine's Docker Engine is effectively root access to the machine. An agent that could reach it could take over the host. Each sandbox gets its own engine instead.

When `agent-app` is running inside the sandbox, click **Check**.