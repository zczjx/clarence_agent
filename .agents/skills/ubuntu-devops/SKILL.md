---
name: ubuntu-devops
description: Ubuntu 本地开发机运维速查。用于 systemd 服务、日志、资源、网络监听与进程排查；优先只读命令。
metadata: {"openclaw": {"os": ["linux"], "emoji": "🐧"}}
---

# Ubuntu 开发机运维（只读优先）

在动手前确认：是否需要 `sudo`；若会改系统状态，先征得用户明确同意。

## systemd 与日志

- 列出运行中单元：`systemctl list-units --type=service --state=running`
- 某服务状态：`systemctl status <unit>`（可加 `--no-pager`）
- 近期日志：`journalctl -u <unit> -n 200 --no-pager`
- 按时间窗口：`journalctl -u <unit> --since "1 hour ago"`
- 启动失败时：`journalctl -xb -p err -n 100`

## 资源与磁盘

- 磁盘：`df -hT`；目录占用：`du -sh <path>/*`（大目录慎用）
- 内存：`free -h`
- 负载与进程：`uptime`，`ps aux --sort=-%mem | head -25`

## 网络（本机监听与连接）

- 监听端口：`ss -tulpn`（无权限时可能看不到 PID）
- 路由：`ip route`，地址：`ip -br a`

## 包与内核（只读）

- 已装包查询：`dpkg -l | grep <name>` 或 `apt list --installed 2>/dev/null | grep <name>`
- 内核：`uname -r`

## 输出习惯

- 长输出加 `--no-pager` 或管道到 `| head -n`
- 敏感信息（token、密码）打码后再写回用户
