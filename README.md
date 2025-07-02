# AVD + Containerlab Lab on macOS (Docker-Outside-of-Docker)More actions

Run Arista cEOS **natively on Apple Silicon** using Containerlab’s *docker-outside-of-docker* (DOOD) dev-container.  This repository contains an end-to-end example that:

- Builds an Arista Validated Design (AVD) configuration for a small Clos fabric.
- Deploys the fabric with Containerlab on your local Mac.
- Uses the official **ARM64** `cEOS-lab` image.

> **Why DOOD?**   The DOOD dev-container shares the host’s Docker daemon, so you can run networking containers at full native speed while keeping your tooling (Python, Ansible, AVD, etc.) isolated inside the container.

---

## Table of Contents

- [AVD + Containerlab Lab on macOS (Docker-Outside-of-Docker)More actions](#avd--containerlab-lab-on-macos-docker-outside-of-dockermore-actions)
  - [Table of Contents](#table-of-contents)
  - [Prerequisites](#prerequisites)
  - [Clone \& open the dev-container](#clone--open-the-dev-container)
  - [Import the ARM64 cEOS image](#import-the-arm64-ceos-image)
  - [Install AVD and Ansible dependencies](#install-avd-and-ansible-dependencies)
  - [Generate configs with AVD](#generate-configs-with-avd)
  - [Deploy configs with AVD](#deploy-configs-with-avd)
  - [Deploy the lab](#deploy-the-lab)
  - [Interact with the devices](#interact-with-the-devices)
  - [Destroy / Redeploy](#destroy--redeploy)
  - [Troubleshooting](#troubleshooting)
  - [Extending the topology](#extending-the-topology)
  - [Credits](#credits)

---

## Prerequisites

| Tool               | Version          | Notes                                                   |
| ------------------ | ---------------- | ------------------------------------------------------- |
| **Docker Desktop** | 4.30+            | Or OrbStack / Colima—must expose the host Docker socket |
| **Git**            | latest           | `brew install git`                                      |
| **VS Code**        | latest           | plus the **Dev Containers** extension                   |
| **Containerlab**   | *none on host*   | Pre-installed inside the dev-container                  |
| **cEOS-lab ARM64** | 4.30.2F or newer | Downloaded from Arista Portal, ≈ 650 MB tar             |

> Your Mac must be running macOS 13 or newer on Apple M-series silicon.

---

## Clone & open the dev-container

```bash
# 1. Clone the repository
$ git clone https://github.com/natedoot/avd-clab-dood.git
$ cd avd-clab-dood

# 2. Open in VS Code (this step launches the DOOD dev-container)
$ code .
```

When prompted by VS Code, choose **“Reopen in Container”**.  The image `ghcr.io/srl-labs/containerlab/devcontainer-dood:<version>` will be pulled and started with the necessary `--privileged` and mount options so that Docker networking works from inside the container.

You can verify DOOD is working:

```bash
# Inside the VS Code terminal (dev-container)
$ docker ps   # ← should list the **host** containers
```

---

## Import the ARM64 cEOS image

The Arista cEOS image is distributed as a tarball.  Copy it into the project (or a local cache directory) and load it **once on the host**:

```bash
# From macOS (not inside the dev-container)
$ docker load -i ceos-lab-arm64-4.30.2F.tar
$ docker tag ceos-lab:4.30.2F ceos:latest   # convenience tag
```

The image is now visible both on the host *and* inside the dev-container.

---

## Install AVD and Ansible dependencies

Once inside the dev-container, install AVD and supporting Ansible collections:

```bash
$ pip install "pyavd[ansible]"
$ ansible-galaxy collection install arista.avd
```

This only needs to be done once after launching the container the first time, or whenever the container is rebuilt.

---

## Generate configs with AVD

Inside the dev-container, run:

```bash
$ ansible-playbook playbooks/build.yml
```

This builds the configurations using Arista Validated Designs (AVD) based on the inventory and group\_vars in `./avd/`, rendering EOS configurations into `./configs/`.

## Deploy configs with AVD

```bash
$ ansible-playbook playbooks/deploy-eapi.yml
```
> **Note**: To deploy using CV, export your onboarding token ```bash export LICENSE_FILE_PATH=/cv-onboarding-token.tok```

---

## Deploy the lab

```bash
# Still inside the dev-container
$ sudo containerlab deploy -t dc1.yml
```

- `dc1.yml` defines a 3-stage Clos topology: 2 spines & 4 leafs (adjust to your liking).
- Startup takes \~30 seconds on an M2 Pro.
- Console ports are exposed on **tcp/200x**; credentials default to `admin / admin`.

View the topology graph:

```bash
$ containerlab graph -t dc1.yml --open  # opens an SVG in your browser
```

---

## Interact with the devices

```bash
# Exec into a switch Cli
$ containerlab exec -t dc1.yml -n dc1-leaf1a -- Cli

# Or use SSH (port numbers are auto-printed by the deploy command)
$ ssh admin@127.0.0.1 -p 2001
leaf1# show lldp neighbors
```

Useful shortcuts:

| Command                               | Purpose                             |
| ------------------------------------- | ----------------------------------- |
| `containerlab inspect --topo dc1.yml` | List nodes, mgmt IPs, port mappings |
| `containerlab tools pcaps`            | Start a packet capture on any link  |
| `docker logs <node>`                  | View EOS boot log                   |

---

## Destroy / Redeploy

```bash
# Tear everything down (containers & veth pairs)
$ sudo containerlab destroy -t dc1.yml

# Redeploy after making changes
$ sudo containerlab deploy -t dc1.yml --reconfigure
```

---

## Troubleshooting

- **Docker socket permission denied**: ensure the dev-container is started with `--privileged` and the socket is mounted (`/var/run/docker.sock`).
- **Interfaces not created**: restart Docker Desktop; the underlying VM sometimes drops `veth` namespace hooks.
- **Slow boot / high CPU**: confirm you are using the **ARM64** cEOS build; x86\_64 under Rosetta is painfully slow.
- **cEOS license prompt**: add `command: ["--license", "accept"]` under the node in `dc1.yml`.

---

## Extending the topology

1. Edit `dc1.yml` to add nodes/links.
2. Add per-device vars to `./avd/` if needed.
3. Re-run `ansible-playbook playbooks/build.yml` to regenerate configs.
4. `containerlab deploy -t dc1.yml --reconfigure` to apply.

---

## Credits

- [Containerlab](https://containerlab.dev) for the awesome tooling.
- [Arista AVD](https://avd.arista.com) for declarative fabric configs.
- Adapted from Containerlab’s official DOOD dev-container example.

---
