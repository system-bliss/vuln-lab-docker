# Reality Matrix

This repository distinguishes real vulnerable components from honest source/dependency skeletons.

| Lab | Status | Component Basis |
| --- | --- | --- |
| `CVE-2025-46817-lab` | `real-component` | Redis image `8.2.1` |
| `CVE-2025-67419-lab` | `real-component` | EverShop source `v2.1.0` |
| `CVE-2026-0920-lab` | `real-component` | WordPress `6.5.5`, Elementor, and official `lastudio-element-kit` `1.5.6.3` ZIP |
| `CVE-2026-11462-lab` | `real-source-skeleton` | BeikeShop source tag `v1.6.0.22` is cloned into the image |
| `CVE-2026-2005-lab` | `real-component` | PostgreSQL source commit `4b324845`, pgcrypto vulnerable path |
| `CVE-2026-2058-lab` | `real-component` | CloudClassroom-PHP-Project source commit `5dadec098bfbbf3300d60c3494db3fb95b66e7be` |
| `CVE-2026-22557-lab` | `real-component` | UniFi Network Application image `lscr.io/linuxserver/unifi-network-application:10.1.85` |
| `CVE-2026-24061-lab` | `real-component` | GNU Inetutils source `2.6`, real telnetd with `-f root` bypass |
| `CVE-2026-26988-lab` | `real-component` | LibreNMS image `librenms/librenms:25.12.0` |
| `CVE-2026-27180-lab` | `real-component` | MajorDoMo source ref `41086aaa` |
| `CVE-2026-29000-lab` | `real-dependency-skeleton` | Maven dependency `org.pac4j:pac4j-jwt:6.3.2` |
| `CVE-2026-29053-lab` | `real-component` | Ghost image `ghost:6.19.0` |
| `CVE-2026-32604-lab` | `real-component` | Spinnaker Clouddriver image `ghcr.io/spinnaker/clouddriver:2026.0.0-ubuntu` with lab-local config |
| `CVE-2026-35031-lab` | `real-component` | Jellyfin image `jellyfin/jellyfin:10.10.3` |
| `CVE-2026-3891-lab` | `real-component` | WordPress `6.5.5`, WooCommerce `9.1.4`, and Pix for WooCommerce `1.5.0` |
| `CVE-2026-3960-lab` | `real-component` | H2O-3 `3.46.0.9` runtime with PostgreSQL JDBC driver for the vulnerable flow |
| `CVE-2026-42879-lab` | `real-component` | FacturaScripts build `2025.81` |
| `CVE-2026-44262-lab` | `real-dependency-skeleton` | Composer package `dedoc/scramble:0.13.21` inside a minimal Laravel app |
| `CVE-2026-45347-lab` | `real-component` | Open WebUI image `ghcr.io/open-webui/open-webui:v0.5.10` |
| `CVE-2026-4631-lab` | `lab-assisted-real-component` | Real Cockpit/OpenSSH packages, with a lab patch where distro backports interfere |
| `CVE-2026-4781-lab` | `real-component` | SourceCodester Sales and Inventory System application package |
| `CVE-2026-49261-lab` | `real-component` | MariaDB Server image `11.4.11` with Galera `wsrep_notify_cmd` |
| `CVE-2026-49443-lab` | `real-component` | authentik image `ghcr.io/goauthentik/server:2026.2.3` |
| `CVE-2026-5027-lab` | `real-component` | Langflow image `langflowai/langflow:1.8.4` |
| `CVE-2026-6815-lab` | `real-component` | Casdoor all-in-one image `casbin/casdoor-all-in-one:2.328.0` |

Definitions:

- `real-component`: starts the real affected application, plugin, package runtime, or service version and routes the documented reproduction to the real handler.
- `real-source-skeleton`: the vulnerable source version is present, but full business initialization or exploit wiring still needs work.
- `real-dependency-skeleton`: the vulnerable package version is present in a minimal service, but a complete upstream framework flow still needs work.
- `lab-assisted-real-component`: based on real packages, with a documented lab-only patch or helper where vendor backports or environment constraints would otherwise hide the vulnerable path.

Documentation rule:

Every lab README must state whether it is a full reproduction, a source/dependency skeleton, or a lab-assisted reproduction. Version banners, static fixtures, and mock endpoints are not enough to claim exploit success.
