---
title: "Skill: 归零法则 (Ex Nihilo)"
author: nio (HM) & opencode (First Mate)
---

# Skill: 归零法则 (Ex Nihilo)

## 技能描述 (Skill Description)
在处理长期运行的系统、复杂的微服务或包含大量本地状态 (Volumes) 的边缘节点时，系统状态会不可逆地走向混乱（熵增）。本技能教导 AI 智能体如何**用清理代替调试**，实现安全可靠的系统自愈。

## 触发场景 (When to use)
*   当你尝试通过 Terminal 分析一个由于旧缓存、脏数据引发的奇怪报错（如 Containerd 的 `blob not found`，或 Matrix 的 Token 同步错乱），且排查成本过高时。
*   当系统因为长期的迭代修改导致声明式配置与实际运行状态发生严重偏离时。

## 执行指令 (Actionable Directives)

1.  **停止猜测，承认混沌**：不要被过度复杂的错误日志所欺骗。如果报错呈现出明显的“状态污染”特征，请立即停止逐行 debug。
2.  **清理可变状态 (Destroy Mutable State)**：
    *   果断删除容器挂载的临时 Volume 数据目录（如果数据已妥善备份或本就属于临时层）。
    *   示例命令：`rm -rf /data/docker_lib/*` 或 `docker-compose down -v`
3.  **从基态重建 (Rebuild from Ground State)**：
    *   依赖我们坚不可摧的基础设施代码（IaC）和高度包容的基础镜像。
    *   因为起点的镜像是确定的，通过 CI/CD 流水线或 Ansible Playbook 重新唤醒服务。
4.  **哲学对齐**：越是干净纯粹的业务枝叶，越需要深邃且包容的底层根系。将水土不服的复杂性深埋到底层容器镜像中，业务层只需“无中生有”。

## 技能总结
“当环境的熵增到无法理清时，就删掉它。回到坚固的基础镜像，从零开始。这就是 Ex Nihilo 的哲学。”