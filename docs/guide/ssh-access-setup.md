# SSH Access Setup

## Overview

This guide describes how to establish secure SSH connectivity between the ForgeOps management machine and its target servers.

The objective is to enable centralized server administration through SSH key-based authentication.

## Architecture

- **Management Machine:** Linux Distribution or Ubuntu running under WSL2 (on Windows).
- **Target Servers:** Ubuntu Server 24.04 virtual machines.
- **Authentication:** Ed25519 SSH key pair.
- **Automation User:** `deploy`.

## Prerequisites

- WSL2 with Ubuntu installed. (Only if you are on windows)
- SSH client available on the management machine.
- Ubuntu target servers with OpenSSH Server installed.
- Network connectivity between the management machine and target servers.

## 1. Generate SSH Key Pair

Generate a dedicated Ed25519 key pair on the management machine:

```bash
ssh-keygen -t ed25519 -C "forgeops-management"
```

The generated files are:

- `~/.ssh/id_ed25519`: Private key.
- `~/.ssh/id_ed25519.pub`: Public key.

The private key must remain on the management machine and must never be committed to the repository.

## 2. Configure Automation User

Create a dedicated automation user on each target server:

```bash
sudo adduser deploy
sudo usermod -aG sudo deploy
```

This configuration is intended for the initial laboratory environment. Production deployments should apply least-privilege permissions.

## 3. Distribute Public Key

Copy the public key to each target server:

```bash
ssh-copy-id -i ~/.ssh/id_ed25519.pub deploy@SERVER_IP
```

Repeat for every managed server.

## 4. Configure SSH Aliases

Edit `~/.ssh/config` on the management machine:

```sshconfig
Host forgeops-node-01
    HostName SERVER_IP
    User deploy
    IdentityFile ~/.ssh/id_ed25519

Host forgeops-node-02
    HostName SERVER_IP
    User deploy
    IdentityFile ~/.ssh/id_ed25519
```

Set appropriate permissions:

```bash
chmod 600 ~/.ssh/config
```

## 5. Validate Connectivity

Test each configured host:

```bash
ssh forgeops-node-01
ssh forgeops-node-02
```

Verify that the authenticated user is `deploy`:

```bash
whoami
```

## Security Considerations

- Never store private SSH keys in Git.
- Use dedicated automation identities.
- Restrict administrative privileges.
- Do not disable password authentication until key-based access has been verified.
- Production environments should restrict SSH access through firewall rules and appropriate network controls.

## Status

SSH key-based authentication successfully validated in the ForgeOps laboratory.