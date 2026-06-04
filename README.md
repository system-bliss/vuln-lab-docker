# vuln-lab-docker

CVE-2026 series Docker vulnerability labs for local, isolated reproduction and security research.

This repository collects Docker-based vulnerable environments for CVE-2026 proof-of-concept verification. Each lab is self-contained and includes its own Docker Compose configuration, startup scripts, and reproduction notes.

> For authorized security testing, vulnerability research, and local lab use only. Do not expose these services to the public internet.

## Labs

| Lab | Target | Main service | Default ports | Notes |
| --- | --- | --- | --- | --- |
| `CVE-2026-26988-lab` | LibreNMS | Web + MariaDB + Redis | `8000/tcp` | Authenticated SQL injection timing lab |
| `CVE-2026-27180-lab` | MajorDoMo | Web + MariaDB | `8899/tcp` | Update flow reproduction lab |
| `CVE-2026-4631-lab` | Cockpit | Cockpit + SSH | `9090/tcp`, `2222/tcp` | Timing-based RCE reproduction lab |

## Requirements

- Docker Engine
- Docker Compose v2
- Python 3 for helper scripts
- Linux, WSL, or another Docker-capable environment

On Windows, WSL is recommended for running the labs. If a service is running inside WSL, get the WSL IP with:

```powershell
wsl hostname -I
```

## Quick Start

Clone the repository:

```bash
git clone https://github.com/system-bliss/vuln-lab-docker.git
cd vuln-lab-docker
```

Start a lab:

```bash
cd CVE-2026-26988-lab
docker compose up -d --build
```

View logs:

```bash
docker compose logs -f
```

Stop and clean up:

```bash
docker compose down
```

Some labs include additional scripts such as `start.sh`, `start.ps1`, `setup.sh`, or Python verification scripts. See the README inside each lab directory for the detailed reproduction flow.

## Repository Layout

```text
.
|-- CVE-2026-26988-lab/
|-- CVE-2026-27180-lab/
|-- CVE-2026-4631-lab/
|-- .gitignore
`-- README.md
```

## Safety Notes

- Run labs only in isolated local or authorized test environments.
- Do not bind vulnerable services to public network interfaces unless the environment is fully controlled.
- Remove containers, images, and generated data after testing.
- Review each lab's scripts before use, especially PoC and verification helpers.

## Disclaimer

The content in this repository is provided for educational and defensive security research purposes. Users are responsible for complying with all applicable laws, rules, and authorization boundaries.
