# Using Ansible

## Overview

This guide explains how ForgeOps uses Ansible to configure servers from one machine, without installing an agent on those servers.

Ansible runs on the **control node** (the management machine, including WSL2). It connects over SSH to the **managed nodes** (the Ubuntu servers) and applies the state described in a playbook. The same playbook can run many times. If a server is already in the desired state, Ansible reports OK and does not change it. That property is idempotency.

SSH access is a prerequisite. Follow [SSH Access Setup](ssh-access-setup.md) before the first playbook run.

Run every command in this guide from `configuration/ansible`, with the virtual environment active. `ansible.cfg` lives in that directory and points Ansible at the inventory.

## What each piece is

| Piece | What it is | Where it lives in ForgeOps |
|---|---|---|
| Control node | Machine where you run Ansible | Your management machine |
| Managed node | Server Ansible configures | Each host in the inventory |
| Inventory | List of managed nodes, groups, and connection settings | `configuration/ansible/inventory/host.ini` |
| Group | A name for a set of hosts, such as `servers` | A `[servers]` section in the inventory |
| Host | One managed node inside a group | A line under that section |
| Group variables | Settings applied to every host in a group | `[servers:vars]` |
| `ansible.cfg` | Project defaults, including which inventory file to use | `configuration/ansible/ansible.cfg` |
| Module | The small program that does one job on the node (`ping`, `debug`, `apt`) | Called from a task; Ansible copies it over SSH and removes it afterward |
| Task | One named action inside a play | An item under `tasks:` |
| Play | One block that selects hosts and runs tasks on them | One `- name:` entry at the root of a playbook |
| Playbook | YAML file with one or more plays | `configuration/ansible/playbooks/` |
| Facts | System details Ansible collects before the tasks (`hostname`, OS, architecture) | Available as `ansible_facts` |
| Plugin | An extension of Ansible itself, such as the SSH connection | Built in; not a file in this repo yet |

A **role** is a later packaging step: tasks, files, and handlers grouped under a name so several playbooks can reuse them. ForgeOps does not use roles yet. Playbooks are enough for the current lab.

## Repository layout

```text
configuration/ansible/
├── ansible.cfg              # local; not committed
├── ansible.cfg.example
├── requirements.txt
├── inventory/
│   ├── host.ini             # local; not committed
│   └── host.ini.example
└── playbooks/
    └── system-information.yml
```

`host.ini` and `ansible.cfg` contain the real hosts, user, and key path. Copy each `*.example` file, remove the `.example` suffix, and fill in the local values. Commit the examples only.

## Inventory

The inventory answers three questions: which machines exist, how to reach them, and which name a playbook uses to select them.

`inventory/host.ini.example`:

```ini
[servers]
node-01 ansible_host=<IP_ROUTE>

[servers:vars]
ansible_user=<USER>
ansible_ssh_private_key_file=<FILE PATH>
```

- `[servers]` creates the group. A playbook that says `hosts: servers` runs on every host in this section.
- `node-01` is the inventory name. Use it with `--limit` and in the play recap. It does not have to be the DNS name.
- `ansible_host` is the address SSH actually connects to.
- `[servers:vars]` applies to the group `servers`. The name must match the group. `[server:vars]` would not apply to `[servers]`.
- `ansible_user` is the SSH user from the access guide.
- `ansible_ssh_private_key_file` is the private key on the control node. It stays on that machine.

A second host is another line in the same group:

```ini
[servers]
node-01 ansible_host=<IP_ROUTE>
node-02 ansible_host=<IP_ROUTE>
```

## ansible.cfg

```ini
[defaults]
inventory = inventory/host.ini
```

`inventory` is a path relative to `ansible.cfg`. With this set, `ansible` and `ansible-playbook` use `inventory/host.ini` without an `-i` flag.

## Playbook YAML

A playbook is a YAML list. Each item in that list is a play.

`playbooks/system-information.yml`:

```yaml
- name: System Information
  hosts: servers

  tasks:
    - name: Get system information
      debug:
        msg:
          - "Hostname: {{ ansible_facts.hostname }}"
          - "Distribution: {{ ansible_facts.distribution }}"
          - "Version: {{ ansible_facts.distribution_version }}"
          - "Architecture: {{ ansible_facts.architecture }}"
```

Read it from the outside in:

1. `- name: System Information` starts a play. The leading `-` is the list item.
2. `hosts: servers` selects the inventory group. `hosts: node-01` would select one host. `hosts: all` selects every host in the inventory.
3. `tasks:` is the list of actions for that play.
4. `- name: Get system information` is one task. The name is what Ansible prints while it runs.
5. `debug:` is the module. Keys nested under it are arguments to that module.
6. `msg` is the argument. The value is a list of four strings.
7. `{{ ansible_facts.hostname }}` is Jinja2. Ansible replaces it with the fact gathered from that host before the task runs.

Indentation is part of the syntax. Use spaces. `tasks` belongs to the play. `debug` belongs to the task. `msg` belongs to `debug`.

Facts are collected because `gather_facts` defaults to `true`. The play recap shows `ok=2` for this file: one OK for fact gathering, and one OK for the `debug` task. `changed=0` because printing facts does not modify the server.

A task that does change a server has the same shape. The module name changes, and its arguments describe the desired state:

```yaml
- name: Install and start nginx
  hosts: servers
  become: true

  tasks:
    - name: Install nginx
      ansible.builtin.apt:
        name: nginx
        state: present
        update_cache: true

    - name: Ensure nginx is running
      ansible.builtin.service:
        name: nginx
        state: started
        enabled: true
```

`become: true` runs the tasks with privilege escalation (`sudo`). `state: present` and `state: started` are idempotent: the second run finds nginx installed and running, so those tasks come back OK instead of changed.

`ansible.builtin.apt` is the fully qualified module name. `apt` and `debug` are the short names of builtin modules. Prefer the full name in new playbooks.

## Commands

Create the virtual environment once:

```bash
cd configuration/ansible
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
```

Check that the control node is using this project:

```bash
ansible --version
```

The reported config file should be `configuration/ansible/ansible.cfg`.

Inspect the inventory before contacting any server:

```bash
ansible-inventory --graph
ansible-inventory --list
ansible servers --list-hosts
```

`--graph` prints groups and hosts. `--list` also shows variables such as `ansible_user` and `ansible_host`. `--list-hosts` only prints the hosts a pattern selects.

Check SSH and Python on the managed nodes:

```bash
ansible servers -m ping
```

`-m ping` is an ad-hoc command: one module, no playbook. A success looks like `"ping": "pong"`.

Run one command without saving a playbook:

```bash
ansible servers -m command -a "uptime"
ansible node-01 -m command -a "uptime"
```

`-m` is the module. `-a` is its arguments. `node-01` limits the run to that inventory name.

Run the playbook:

```bash
ansible-playbook playbooks/system-information.yml
```

Useful variants:

```bash
ansible-playbook playbooks/system-information.yml --syntax-check
ansible-playbook playbooks/system-information.yml --check
ansible-playbook playbooks/system-information.yml --limit node-01
ansible-playbook playbooks/system-information.yml -v
```

- `--syntax-check` parses the YAML and stops. It does not connect.
- `--check` connects and predicts changes without applying them. Modules that cannot check, such as `command`, may still run or be skipped.
- `--limit` restricts the play to a host or group already selected by `hosts:`.
- `-v` prints more detail. `-vvv` is enough for most connection problems.

## What happens on a run

1. Ansible reads `ansible.cfg` and opens `inventory/host.ini`.
2. The play's `hosts:` value selects matching hosts.
3. Ansible opens an SSH connection as `ansible_user`, with the configured private key.
4. It gathers facts, unless the play sets `gather_facts: false`.
5. For each task, it copies the module to the node, runs it, and deletes it.
6. It prints a result per task and a recap per host.

## How to read the recap

```text
node-01 : ok=2  changed=0  unreachable=0  failed=0  skipped=0  rescued=0  ignored=0
```

| Field | Meaning |
|---|---|
| ok | Task finished and the node already matched the desired state, or the task only read data |
| changed | Task finished and Ansible modified the node |
| unreachable | SSH or the network failed before the task could run |
| failed | The task ran and returned an error |
| skipped | A `when:` condition was false, or the task was skipped in check mode |
| rescued | A failure was caught by a `rescue` block |
| ignored | The task failed and was marked `ignore_errors: true` |

`changed=0` on a second run is the usual sign that the playbook is idempotent.

## What stays off Git

Do not commit `inventory/host.ini` or `ansible.cfg`. They hold the lab addresses, the SSH user, and the key path. The files to commit are `host.ini.example`, `ansible.cfg.example`, `requirements.txt`, and the playbooks.
