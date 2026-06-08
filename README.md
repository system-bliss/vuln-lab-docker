# vuln-lab-docker

Local Docker labs for security research and authorized testing.

The important correction in this revision: the repository no longer treats lightweight Python endpoint mocks as real CVE labs. Each lab is now marked by how real its component base is. See `REALITY.md` for the full matrix.

## Lab Matrix

| Lab | Product | Status | Default Port |
| --- | --- | --- | --- |
| `CVE-2026-0920-lab` | WordPress LA-Studio Element Kit | real WordPress plugin `1.5.6.3` | `8103` |
| `CVE-2026-11462-lab` | BeikeShop | real source skeleton `v1.6.0.22` | `8110` |
| `CVE-2026-22557-lab` | UniFi Network Application | real image `10.1.85` | `8101` |
| `CVE-2026-26988-lab` | LibreNMS | real image `25.12.0` | `8000` |
| `CVE-2026-27180-lab` | MajorDoMo | real source ref `41086aaa` | `8899` |
| `CVE-2026-29000-lab` | pac4j-jwt | real dependency skeleton `6.3.2` | `8104` |
| `CVE-2026-44262-lab` | dedoc/scramble | real dependency skeleton `0.13.21` | `8106` |
| `CVE-2026-45347-lab` | Open WebUI | real image `v0.5.10` | `8105` |
| `CVE-2026-4631-lab` | Cockpit | lab-assisted real packages | `9090`, `2222` |
| `CVE-2026-49443-lab` | authentik | real image `2026.2.3` | `8107` |

## Usage

For labs with `docker-compose.yml`:

```bash
cd CVE-2026-0920-lab
docker compose up -d --build
```

## Requirements

- Docker Engine or Docker Desktop
- Docker Compose v2
- Python 3 for helper PoC/check scripts

## Safety

Run these labs only in local isolated or authorized environments. Do not expose vulnerable services to the public internet.

## Verification Scope

Some directories are complete real-component labs; others are honest skeletons that only prove the vulnerable version is present. The skeletons are intentionally not fake exploit services. Complete exploit workflows should be built on top of the real component/dependency already pinned in each directory.
