# vuln-lab-docker 漏洞靶场集合

本仓库提供一组基于 Docker / Docker Compose 的本地漏洞靶场，用于安全研究、授权测试和复现流程学习。

仅建议在本地隔离环境或明确授权的测试环境中使用，不要将这些脆弱服务暴露到公网。

## 靶场矩阵

| 靶场 | 产品/组件 | 真实性状态 | 默认端口 |
| --- | --- | --- | --- |
| `CVE-2026-0920-lab` | WordPress LA-Studio Element Kit | 真实 WordPress 插件 `1.5.6.3` | `8103` |
| `CVE-2026-11462-lab` | BeikeShop | 真实源码骨架 `v1.6.0.22` | `8110` |
| `CVE-2026-22557-lab` | UniFi Network Application | 真实镜像 `10.1.85` | `8101` |
| `CVE-2026-26988-lab` | LibreNMS | 真实镜像 `25.12.0` | `8000` |
| `CVE-2026-27180-lab` | MajorDoMo | 真实源码 ref `41086aaa` | `8899` |
| `CVE-2026-29000-lab` | pac4j-jwt | 真实依赖骨架 `6.3.2` | `8104` |
| `CVE-2026-44262-lab` | dedoc/scramble | 真实依赖骨架 `0.13.21` | `8106` |
| `CVE-2026-45347-lab` | Open WebUI | 真实镜像 `v0.5.10` | `8105` |
| `CVE-2026-4631-lab` | Cockpit | lab patch 辅助的真实组件 | `9090`, `2222` |
| `CVE-2026-49443-lab` | authentik | 真实镜像 `2026.2.3` | `8107` |

完整真实性说明见 `REALITY.md`。本仓库不再把轻量 Python mock 当成真实 CVE 靶场；如果某个目录是源码/依赖骨架，README 会明确说明当前可验证范围和完整复现还需要补齐的业务流程。

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

## Python 复现方式

每个靶场目录都提供 Python PoC 或验证脚本，路径通常位于 `poc/` 目录；少数历史靶场使用根目录脚本，例如 LibreNMS 的 `poc_verify.py` 和 Cockpit 的 `verify.py`。

通用执行方式：

```bash
cd /mnt/d/HackerTools/vuln-lab-docker/<LAB_DIR>
python3 poc/<SCRIPT_NAME>.py --target "http://<TARGET_HOST>:<LAB_PORT>"
```

脚本会根据靶场类型完成真实漏洞复现、服务可达性检查、版本确认或完整复现前置条件检查。请以各靶场目录的 README 为准。

## curl 复现方式

每个靶场 README 都提供 curl 复现或验证请求。通用流程是：

```bash
curl -i "http://<TARGET_HOST>:<LAB_PORT>/"
```

对于需要认证、CSRF、HTTPS 自签名证书、回连服务或时间盲注差分的漏洞，各目录 README 会给出对应的 cookie、header、请求体、`-k` 参数或时间差判断方式。

## Burp 复现方式

每个靶场 README 都提供 Burp Suite / Repeater 复现或验证方式。通用流程是：

- 在浏览器或 curl 中访问靶场，将请求发送到 Burp Proxy。
- 把关键请求发送到 Repeater。
- 按对应 README 修改路径、参数、Header、Cookie 或请求体。
- 观察响应内容、响应时间、服务日志或内部回连记录判断漏洞是否触发。

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

仓库内每个靶场 README 均应使用中文说明，并至少包含 Python、curl、Burp 三种复现或验证方式。对于真实源码骨架和真实依赖骨架，文档必须明确说明当前只验证真实组件/依赖存在，不伪造漏洞利用成功结果。
