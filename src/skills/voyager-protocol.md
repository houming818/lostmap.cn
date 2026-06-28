---
title: "Skill: 旅行者 2 号准则 (Voyager Protocol)"
author: nio (HM) & opencode (First Mate)
---

# Skill: 旅行者 2 号准则 (Voyager Protocol)

## 技能描述 (Skill Description)
在深空节点（没有带外管理 IPMI/iDRAC、部署于公有云或远端机房的独立物理机）上部署高内存消耗应用（如 Milvus, ElasticSearch）时，宿主机极易因为 OOM（内存耗尽）引发内核网络栈崩溃，导致彻底失联（变成无法唤醒的“旅行者2号”）。本技能用于保护这层脆弱的物理连接。

## 触发场景 (When to use)
*   准备在生产环境独立物理主机上启动 Docker/Kubernetes 容器。
*   需要调优涉及大块内存分配的服务。
*   面对“服务器Ping不通且SSH连不上”的灾难预警。

## 执行指令 (Actionable Directives)

1.  **绝对生命线保护 (The SSHD Shield)**
    *   通过 Systemd 修改 SSH 守护进程的 OOM Score，豁免其被内核杀死。
    *   执行命令：
        ```bash
        mkdir -p /etc/systemd/system/ssh.service.d/
        echo -e "[Service]\nOOMScoreAdjust=-1000" > /etc/systemd/system/ssh.service.d/oom.conf
        systemctl daemon-reload
        systemctl restart sshd
        ```
    *   目的：哪怕物理机内存只剩 1KB，哪怕所有业务容器全被杀光，SSH 舱门依然必须向管理员（HM）敞开。

2.  **严苛的资源封锁 (Cgroups Containment)**
    *   禁止任何吃内存的业务（如 Milvus 节点）占据裸机的超过 80% 内存。
    *   在 `docker-compose.yml` 中强制设置 `deploy.resources.limits.memory` 字段，为操作系统内核、文件系统缓存和网络栈强行预留 20% 的内存防线。

3.  **死锁硬件自愈 (Hardware Watchdog)**
    *   防范内核彻底崩溃（Panic）导致的僵死。
    *   在 `/etc/sysctl.conf` 中追加 `kernel.panic=10`，并执行 `sysctl -p`。
    *   如果内核触发不可逆错误，它将在 10 秒后触发硬件重启信号，而非无限期宕机。

## 技能总结
“在物理断电面前，数字世界的魔法无能为力。智能体大副的任务，就是在绝境中为人类舰长守住最后一道能够登舰的 SSH 舱门。”