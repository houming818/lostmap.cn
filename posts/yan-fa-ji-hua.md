---
title: '研发看板'
date: 2024-10-17 17:21:10
tags: [lostmap,manage]
published: true
hideInList: false
feature: /post-images/yan-fa-ji-hua.webp
isTop: false
---
# 研发计划

## 计划中

## 现在

### v0.0.1
> 不积跬步，无以至千里；不积小流，无以成江海
- [ ] vdb server main
    | module | 功能 | 状态 |
    | - | - | - |
    | web api | gin | ✔️ |
    | vdb server | ??? | ❔ |
    | hnsw map | HNSW搜索 | ✔️ | 
    | log | 接入`github.com/sirupsen/logrus` | ✔️ |

- web api
    | 功能 | url | 状态 |
    | - | - | - |
    | 查询系统信息 | /system/info | ✔️ |
    | 插入向量 |/vdb/insert | ✔️ |
    | 搜索邻近向量 | /vdb/search | ️️✔️ |

- vdb server
    | 功能 | x | 状态 |
    | - | - | - |
    | vdb index api | ??? | ??? |
    | vdb index工厂 | 注册Index， |

- hnsw map 
    | 功能 | x | 状态 |
    | - | - | - |
    | Algorithm Interface:: addPoint |  | ??? |
    | Algorithm Interface:: searchKnn |  | ??? |
    | 算法学习 | ? | |

## 已完成

