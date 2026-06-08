# Reality Matrix

This repository distinguishes real vulnerable components from honest source/dependency skeletons.

| Lab | Status | Component Basis |
| --- | --- | --- |
| `CVE-2026-0920-lab` | `real-component` | WordPress + Elementor + official `lastudio-element-kit` `1.5.6.3` ZIP |
| `CVE-2026-11462-lab` | `real-source-skeleton` | BeikeShop source tag `v1.6.0.22` is cloned into the image |
| `CVE-2026-22557-lab` | `real-component` | UniFi Network Application image `lscr.io/linuxserver/unifi-network-application:10.1.85` |
| `CVE-2026-26988-lab` | `real-component` | LibreNMS image `25.12.0` |
| `CVE-2026-27180-lab` | `real-component` | MajorDoMo source ref `41086aaa` |
| `CVE-2026-29000-lab` | `real-dependency-skeleton` | Maven dependency `org.pac4j:pac4j-jwt:6.3.2` |
| `CVE-2026-44262-lab` | `real-dependency-skeleton` | Composer package `dedoc/scramble:0.13.21` |
| `CVE-2026-45347-lab` | `real-component` | Open WebUI image `ghcr.io/open-webui/open-webui:v0.5.10` |
| `CVE-2026-4631-lab` | `lab-assisted-real-component` | Real Cockpit/OpenSSH packages, with a lab patch where distro backports interfere |
| `CVE-2026-49443-lab` | `real-component` | authentik image `ghcr.io/goauthentik/server:2026.2.3` |

Definitions:

- `real-component`: starts the real affected application or plugin version.
- `real-source-skeleton`: the vulnerable source version is present, but full business initialization/exploit wiring still needs work.
- `real-dependency-skeleton`: the vulnerable package version is present in a minimal service, but a complete framework flow still needs work.
- `lab-assisted-real-component`: based on real packages, with a documented lab-only patch to keep the vulnerable path reproducible.
