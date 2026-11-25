# Run an AI agent safely: Docker Sandboxes for IT admins

A 35-minute hands-on [Instruqt](https://instruqt.com) lab that shows IT admins, security and DevOps teams how [Docker Sandboxes](https://docs.docker.com/ai/sandboxes/) contains AI coding agents.

**▶ Play the lab:** https://play.instruqt.com/valencecloud/invite/zgm2ydlvvqgs
No sign-up, Docker experience, AI subscription or API key needed.

## What learners do

| # | Challenge | What the learner proves |
|---|---|---|
| 1 | Why sandbox AI agents? | A prompt-injection scenario and a quiz: why an agent's permissions are the real risk |
| 2 | Launch your first sandbox | Starts a microVM with `sbx run` and finds it from the admin's side with `sbx ls` |
| 3 | Prove the isolation | The agent can't see the host's files or containers, and the host can't see the agent's |
| 4 | Lock down the network | Switches to the Locked Down policy, sees a request blocked and logged, then allows one domain |
| 5 | Clean up and wrap up | Removes the sandbox, confirms the project folder survives, and links the lesson to central governance |

Each learner gets two terminals on the same machine: **Sandbox** (the agent's view, inside) and **Admin** (the IT admin's view, outside). Most steps run the same command on both sides, so the wall is visible rather than described.

## Repository layout

```
run-an-ai-agent-safely-docker-sandboxes-for-it-admins/
├── track.yml                     Title, description, tags, 45-minute limit
├── config.yml                    Ubuntu 24.04 VM with nested virtualization, one secret
├── track_scripts/
│   └── setup-lab-machine         Installs Docker Engine and sbx, signs in, sets the Balanced policy
├── 01-why-sandbox-ai-agents/     Quiz
├── 02-launch-your-first-sandbox/ Instructions, check, solve
├── 03-prove-the-isolation/       Instructions, setup, check, solve
├── 04-lock-down-the-network/     Instructions, check, solve
└── 05-clean-up-and-wrap-up/      Instructions, check, solve
```

## Design decisions

- **Real microVMs, not a simulation.** The lab machine is a VM with nested virtualization enabled, so learners run the actual `sbx` CLI.
- **No AI key needed.** Learners use the `shell` agent: the same sandbox, with a terminal instead of an AI model. Swapping `shell` for `claude` or `copilot` changes nothing else.
- **Secure defaults are built in, not just written in the instructions.** Every terminal opens in `/root/project`, so `sbx run ... .` can't accidentally share the home folder that holds the sign-in file.
- **Checks test results, not keystrokes,** and each failure has its own hint. Challenge 4's check also confirms the learner saw a request blocked, not just that the final rule exists. No check can leave a learner in a state they can't recover from.
- **Instruqt for doing, Skilljar for reviewing.** Final review questions, with explanations for each answer, are designed to live in the Skilljar course that hosts the lab.

## Security

- The lab signs in with a **dedicated, read-only Docker account**, never a personal one. Its token is stored as an Instruqt secret (`DOCKER_PAT`) and never appears in this repository.
- The setup script turns off command tracing (`set +x`) around the sign-in, so the token never appears in the track logs. This was verified in the test logs.
- The share link expires before the token does, so reviewers never meet a lab that fails to start.

## Testing

- **Manual:** played end to end as a learner on the target platform. This found and fixed four issues, including terminals that didn't start in the project folder.
- **Automated:** `instruqt track test` runs every challenge's setup, then its check (which must fail), then solve, then check (which must pass). This caught a placeholder check that always passed. Re-run it whenever Docker releases a new `sbx` version.

## Build and test it yourself

Requires the [Instruqt CLI](https://docs.instruqt.com/getting-started/set-up-instruqt), an Instruqt team, and a team secret named `DOCKER_PAT` holding a read-only Docker token. Change the `--username` in `track_scripts/setup-lab-machine` to your lab account.

```bash
cd run-an-ai-agent-safely-docker-sandboxes-for-it-admins
instruqt track push
instruqt track test
```

## Author

Built by **Faiz Khan** ([Valence Cloud](https://valencecloud.com)), as part of an application for the Senior Manager, Training role at Docker.
