# vuln-lab-docker

`vuln-lab-docker` 是一个面向本地安全研究与授权测试的 Docker 漏洞靶场集合。仓库按 CVE 编号组织目录，每个靶场尽量做到独立、可一键部署、可复现、可清理，方便在隔离环境中验证漏洞触发链路、编写检测规则、训练复现流程。

> 仅允许在本地隔离环境、授权测试环境、教学或防御研究场景中使用。请勿将任何靶场服务暴露到公网，也不要对未授权目标使用本仓库中的复现脚本。

## 项目特点

- 每个 CVE 独立目录，包含 `Dockerfile`、`docker-compose.yml`、靶场说明和复现步骤。
- 多数靶场支持 `setup.sh`、`start.sh` 或 `docker compose up -d --build` 一键启动。
- 新增轻量 Python 靶场统一支持 Python 脚本、curl、Burp Repeater 三种复现方式。
- 支持 Windows + WSL + Docker Desktop 的本地复现实验环境。
- 所有示例 Payload 都约束在本地靶场范围内，避免误读宿主机敏感文件或执行任意危险命令。

## 靶场清单

| 靶场目录 | 影响产品 | 漏洞类型 | 默认端口 | 复现方式 |
| --- | --- | --- | --- | --- |
| `CVE-2026-0920-lab` | WordPress LA-Studio Element Kit | 权限绕过 / 未授权创建管理员 | `8103` | Python / curl / Burp |
| `CVE-2026-11412-lab` | Jinher OA / 金和 OA | SQL 时间盲注 | `8102` | Python / curl / Burp |
| `CVE-2026-11456-lab` | Chanjet CRM | SQL 时间盲注 | `8109` | Python / curl / Burp |
| `CVE-2026-11462-lab` | BeikeShop | Stripe 回调签名绕过 | `8110` | Python / curl / Burp |
| `CVE-2026-21992-lab` | Oracle Identity Manager | 未授权代码执行面 | `8108` | Python / curl / Burp |
| `CVE-2026-22557-lab` | Ubiquiti UniFi | 路径穿越 | `8101` | Python / curl / Burp |
| `CVE-2026-26988-lab` | LibreNMS | 认证后 SQL 注入 | `8000` | README 内步骤 / PoC |
| `CVE-2026-27180-lab` | MajorDoMo | 更新流程漏洞复现 | `8899` | Python 攻击服务 / curl / Burp |
| `CVE-2026-29000-lab` | pac4j-jwt | JWT 签名校验绕过 | `8104` | Python / curl / Burp |
| `CVE-2026-44262-lab` | dedoc/scramble Laravel | 受控代码执行模拟 | `8106` | Python / curl / Burp |
| `CVE-2026-45347-lab` | Open WebUI | 盲 SSRF | `8105` | Python / curl / Burp |
| `CVE-2026-4631-lab` | Cockpit | Timing-based RCE 复现 | `9090`, `2222` | README 内步骤 / verify.py |
| `CVE-2026-49443-lab` | authentik | 对象级权限绕过 | `8107` | Python / curl / Burp |

## 环境要求

- Docker Engine 或 Docker Desktop
- Docker Compose v2
- Python 3，用于运行辅助 PoC 或验证脚本
- Linux、WSL、macOS 或其他支持 Docker 的环境

Windows 用户推荐使用 WSL 作为靶场运行环境。获取 WSL IP：

```powershell
wsl hostname -I
```

如果 Docker Desktop 使用 mirrored networking，通常也可以直接访问：

```text
http://127.0.0.1:<PORT>
```

## 快速开始

克隆仓库：

```bash
git clone https://github.com/system-bliss/vuln-lab-docker.git
cd vuln-lab-docker
```

进入某个靶场目录并启动：

```bash
cd CVE-2026-22557-lab
docker compose up -d --build
```

如果目录提供 `setup.sh`，也可以使用：

```bash
chmod +x setup.sh
./setup.sh
```

查看容器状态：

```bash
docker compose ps
```

查看日志：

```bash
docker compose logs -f
```

停止并清理当前靶场：

```bash
docker compose down
```

如需删除数据卷：

```bash
docker compose down -v
```

## 端口与镜像配置

部分轻量 Python 靶场支持通过环境变量修改映射端口：

```bash
LAB_PORT=18101 docker compose up -d --build
```

若当前网络访问 Docker Hub 不稳定，新增 Python 靶场默认使用已验证可拉取的 Python Alpine 镜像源。也可以手动切换回官方镜像：

```bash
PYTHON_IMAGE=python:3.12-alpine docker compose up -d --build
```

## 目录结构

```text
.
|-- CVE-2026-0920-lab/
|-- CVE-2026-11412-lab/
|-- CVE-2026-11456-lab/
|-- CVE-2026-11462-lab/
|-- CVE-2026-21992-lab/
|-- CVE-2026-22557-lab/
|-- CVE-2026-26988-lab/
|-- CVE-2026-27180-lab/
|-- CVE-2026-29000-lab/
|-- CVE-2026-44262-lab/
|-- CVE-2026-45347-lab/
|-- CVE-2026-4631-lab/
|-- CVE-2026-49443-lab/
|-- .gitignore
`-- README.md
```

典型靶场目录包含：

```text
CVE-xxxx-xxxx-lab/
|-- README.md
|-- Dockerfile
|-- docker-compose.yml
|-- setup.sh
`-- poc/
    `-- cve_xxxx_xxxx_exp.py
```

## 复现建议

1. 先阅读对应靶场目录下的 `README.md`，确认默认端口、认证要求和阳性判断。
2. 使用 `docker compose up -d --build` 或 `setup.sh` 启动靶场。
3. 先访问首页或健康接口确认服务可达。
4. 优先运行目录中的 Python PoC 做自动化验证。
5. 再使用 curl 或 Burp Repeater 手工复现，观察请求、响应和日志。
6. 完成后执行 `docker compose down` 清理环境。

## 已验证情况

2026-06-08 新增的 10 个轻量靶场已完成本地实际验证：

```text
CVE-2026-22557-lab  8101  PASS
CVE-2026-11412-lab  8102  PASS
CVE-2026-0920-lab   8103  PASS
CVE-2026-29000-lab  8104  PASS
CVE-2026-45347-lab  8105  PASS
CVE-2026-44262-lab  8106  PASS
CVE-2026-49443-lab  8107  PASS
CVE-2026-21992-lab  8108  PASS
CVE-2026-11456-lab  8109  PASS
CVE-2026-11462-lab  8110  PASS
```

验证内容包括：

- `docker compose config` 解析通过
- `docker compose up -d --build` 构建并启动成功
- HTTP 服务可访问
- 对应 Python PoC 复现成功
- `setup.sh` 通过 WSL `bash -n` 语法检查

## 安全说明

- 请只在本地、隔离、授权环境中运行靶场。
- 不要把靶场端口映射到公网 IP 或云主机公网安全组。
- 不要将 PoC 直接用于未授权资产。
- 部分靶场会模拟命令执行、路径穿越、SSRF、SQL 注入等行为；实现中已尽量限制影响范围，但仍建议在临时测试环境中运行。
- 测试结束后及时清理容器、网络和数据卷。

## 免责声明

本仓库内容仅用于安全研究、漏洞复现学习、防御验证和授权测试。使用者需要自行确保行为符合所在地法律法规、授权边界和组织安全规范。因不当使用造成的任何后果由使用者自行承担。
