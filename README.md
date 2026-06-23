# vuln-lab-docker 漏洞靶场集合

本仓库提供一组基于 Docker / Docker Compose 的本地漏洞靶场，用于安全研究、授权测试和复现流程学习。

仅建议在本地隔离环境或明确授权的测试环境中使用，不要将这些脆弱服务暴露到公网。

## 靶场矩阵

| 靶场 | 产品/组件 | 真实性状态 | 默认端口 |
| --- | --- | --- | --- |
| [`CVE-2025-46817-lab`](./CVE-2025-46817-lab/) | Redis | `real-component`，镜像 `8.2.1` | `8132` |
| [`CVE-2025-67419-lab`](./CVE-2025-67419-lab/) | EverShop | `real-component`，源码 `v2.1.0` | `8134` |
| [`CVE-2026-0920-lab`](./CVE-2026-0920-lab/) | WordPress + LA-Studio Element Kit | `real-component`，插件 `1.5.6.3` | `8103` |
| [`CVE-2026-11462-lab`](./CVE-2026-11462-lab/) | BeikeShop | `real-source-skeleton`，源码 tag `v1.6.0.22` | `8110` |
| [`CVE-2026-2005-lab`](./CVE-2026-2005-lab/) | PostgreSQL pgcrypto | `real-component`，源码 `16.11` | `8133` |
| [`CVE-2026-2058-lab`](./CVE-2026-2058-lab/) | CloudClassroom-PHP-Project | `real-component`，源码 commit `5dadec098` | `8126` |
| [`CVE-2026-22557-lab`](./CVE-2026-22557-lab/) | UniFi Network Application | `real-component`，镜像 `10.1.85` | `8101`，inform `8080` |
| [`CVE-2026-24061-lab`](./CVE-2026-24061-lab/) | GNU Inetutils telnetd | `real-component`，源码 `2.6` | `8128` |
| [`CVE-2026-26988-lab`](./CVE-2026-26988-lab/) | LibreNMS | `real-component`，镜像 `25.12.0` | `8000` |
| [`CVE-2026-27180-lab`](./CVE-2026-27180-lab/) | MajorDoMo | `real-component`，源码 ref `41086aaa` | `8899` |
| [`CVE-2026-29000-lab`](./CVE-2026-29000-lab/) | pac4j-jwt | `real-dependency-skeleton`，Maven 依赖 `6.3.2` | `8104` |
| [`CVE-2026-29053-lab`](./CVE-2026-29053-lab/) | Ghost | `real-component`，镜像 `6.19.0` | `8132` |
| [`CVE-2026-32604-lab`](./CVE-2026-32604-lab/) | Spinnaker Clouddriver | `real-component`，镜像 `2026.0.0-ubuntu` | `8121` |
| [`CVE-2026-35031-lab`](./CVE-2026-35031-lab/) | Jellyfin | `real-component`，镜像 `10.10.3` | `8112` |
| [`CVE-2026-3891-lab`](./CVE-2026-3891-lab/) | WordPress + WooCommerce + Pix for WooCommerce | `real-component`，插件 `1.5.0` | `8111` |
| [`CVE-2026-3960-lab`](./CVE-2026-3960-lab/) | H2O-3 | `real-component`，版本 `3.46.0.9` | `8122`，callback `8123` |
| [`CVE-2026-42879-lab`](./CVE-2026-42879-lab/) | FacturaScripts | `real-component`，版本 `2025.81` | `8114` |
| [`CVE-2026-44262-lab`](./CVE-2026-44262-lab/) | dedoc/scramble | `real-dependency-skeleton`，Composer 包 `0.13.21` | `8106` |
| [`CVE-2026-45347-lab`](./CVE-2026-45347-lab/) | Open WebUI | `real-component`，镜像 `v0.5.10` | `8105` |
| [`CVE-2026-4631-lab`](./CVE-2026-4631-lab/) | Cockpit + OpenSSH | `lab-assisted-real-component`，Cockpit `356.2` + OpenSSH `8.7p1` | `9090`，SSH `2222` |
| [`CVE-2026-4781-lab`](./CVE-2026-4781-lab/) | Sales and Inventory System | `real-component`，SourceCodester 应用包 | `8127` |
| [`CVE-2026-49261-lab`](./CVE-2026-49261-lab/) | MariaDB Galera | `real-component`，镜像 `11.4.11` | `8131` |
| [`CVE-2026-49443-lab`](./CVE-2026-49443-lab/) | authentik | `real-component`，镜像 `2026.2.3` | `8107` |
| [`CVE-2026-5027-lab`](./CVE-2026-5027-lab/) | Langflow | `real-component`，镜像 `1.8.4` | `8120` |
| [`CVE-2026-6815-lab`](./CVE-2026-6815-lab/) | Casdoor | `real-component`，镜像 `2.328.0` | `8113` |

完整真实性说明见 [`REALITY.md`](./REALITY.md)。本仓库不把轻量 mock 当成真实 CVE 靶场；如果某个目录是源码/依赖骨架，README 必须明确说明当前可验证范围，以及完整业务流复现还需要补齐的前置条件。

## 环境说明

推荐拓扑：

- 复现操作环境：Windows
- 靶场运行环境：WSL + Docker / Docker Compose
- Windows 访问方式：`http://<WSL_IP>:<LAB_PORT>`

在 Windows PowerShell 中获取 WSL IP：

```powershell
wsl hostname -I
```

如果 WSL 使用 mirrored networking，Windows 侧也可能直接访问：

```text
http://127.0.0.1:<LAB_PORT>
```

## 通用部署方式

进入任意靶场目录后启动：

```bash
cd /mnt/d/HackerTools/vuln-lab-docker/CVE-2026-0920-lab
chmod +x setup.sh
./setup.sh
```

也可以直接使用 Docker Compose：

```bash
cd /mnt/d/HackerTools/vuln-lab-docker/CVE-2026-0920-lab
docker compose up -d --build
```

修改默认端口：

```bash
LAB_PORT=9001 docker compose up -d --build
```

## Burp 复现方式

每个靶场 README 都提供 Burp Suite / Repeater 复现或验证方式。通用流程是：

- 在浏览器中访问靶场，将请求发送到 Burp Proxy。
- 把关键请求发送到 Repeater。
- 按对应 README 修改路径、参数、Header、Cookie 或请求体。
- 观察响应内容、响应时间、服务日志、证明文件或内部回连记录判断漏洞是否触发。

## 常用排错

查看容器状态：

```bash
docker compose ps
```

查看服务日志：

```bash
docker compose logs -f
```

Windows 无法访问 WSL 内靶场时，重新获取 WSL IP：

```powershell
wsl hostname -I
```

端口被占用时，换端口启动：

```bash
LAB_PORT=9001 docker compose up -d --build
```

## 清理靶场

停止容器：

```bash
docker compose down
```

删除数据卷并重新构建：

```bash
docker compose down -v
docker compose up -d --build
```

## 文档质量约定

仓库内每个靶场 README 均应使用中文说明，并以 Burp Suite / Repeater 作为主要复现或验证方式。README 至少应包含 CVE ID、受影响组件与版本、复现范围、启动命令、目标地址、关键 HTTP 请求、阳性/阴性判断、排错、清理和参考链接。

仅当复现链路必须由服务端脚本托管升级包、feed、回连监听等内容时，才保留对应辅助脚本；纯自动化验证脚本不作为必需文件保留。对于真实源码骨架和真实依赖骨架，文档必须明确说明当前只验证真实组件/依赖存在，不伪造漏洞利用成功结果。
