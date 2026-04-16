---
name: safe-shell
description: 在 Linux 上安全执行 shell 的边界：默认只读；破坏性/提权/包与网络变更前必须确认；适用于本仓库所有运维操作。
metadata: {"openclaw": {"os": ["linux"], "emoji": "🛡️"}}
---

# 安全执行边界

## 默认假设

- **先解释再执行**：说明命令目的与影响范围。
- **默认只读**：优先 `cat`/`ls`/`ss`/`journalctl`/`systemctl status` 等。
- **最小权限**：需要时再 `sudo`；不要建议无意义的 `chmod 777`。

## 执行前必须停下询问用户

- 文件/数据：`rm`、`dd`、覆盖配置、清空日志、数据库 `DROP`
- 系统与包：`apt install/remove`、`dpkg -i`、`snap` 变更
- 服务：`systemctl stop/disable`、`kill -9` 大面积
- 网络：改防火墙、改 `iptables`/`nft`、改路由
- 用户/权限：`useradd`、`chown` 递归系统目录

## 可以主动做的（仍优先只读）

- 读配置：`/etc` 下与问题相关的文件（避免上传密钥）。
- 诊断：只读的 `journalctl`、`ss`、`df`、`ps`。

## 输出

- 若命令失败，附上**完整错误末几行**与**退出码**（若可得）。
- 涉及路径时使用绝对路径或明确 cwd。
