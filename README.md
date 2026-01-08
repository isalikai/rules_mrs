# Mihomo Rule Set (.mrs) Mirror

[![Update All Mihomo Rules](https://github.com/isalikai/rules_mrs/actions/workflows/update-rules.yml/badge.svg)](https://github.com/isalikai/rules_mrs/actions/workflows/update-rules.yml)

本项目用于 **自动同步、转换并维护 Mihomo 可用的 `.mrs` 规则集**，规则来源明确、结构清晰，分为：

- **上游规则镜像（rules/）**
- **自用规则集（self/，基于明确规则源整理）**

所有规则均通过 GitHub Actions **每日自动更新**。

------

## ✨ 项目特性

- ✅ 自动同步上游规则仓库
- ✅ 转换为 Mihomo `.mrs` 二进制格式
- ✅ 保持原始目录结构
- ✅ 支持 Domain / IP 规则
- ✅ 自用规则同样标注规则来源，便于审计与维护

------

## 📂 目录结构说明

### 1️⃣ `rules/` —— 上游规则镜像（默认规则集）

`rules/` 目录用于 **完整镜像并转换** 以下上游仓库中的规则：

- 📦 **规则源**：
  [Accademia/Additional_Rule_For_Clash](https://github.com/Accademia/Additional_Rule_For_Clash)

特点：

- ✔ 与上游仓库目录结构完全一致
- ✔ 仅做格式转换（yaml → mrs）
- ✔ 不对规则内容做任何主观修改
- ✔ 适合通用、公开引用

#### 示例

| 上游文件                          | 本仓库                                 |
| --------------------------------- | -------------------------------------- |
| `GeositeCN/GeositeCN_Domain.yaml` | `rules/GeositeCN/GeositeCN_Domain.mrs` |

------

### 2️⃣ `self/` —— 自用规则集

`self/` 目录用于存放 **个人实际使用的精简 / 定制规则**，
但 **每一条规则都来自可追溯的上游规则源或官方服务域名**，并非凭空编写。

#### 规则来源原则

- 🔎 **明确标注规则参考或来源**
- 🔎 规则内容基于上游规则的**裁剪、合并或精简**
- 🔎 不做来源不明、不可验证的规则
- 🔎 更关注“稳定命中”和“实际可用性”

#### 当前已包含规则源示例

- 📦 **Google Voice**
  - 上游规则参考：
    [blackmatrix7/ios_rule_script](https://github.com/blackmatrix7/ios_rule_script)
  - 原始规则文件：
    `rule/Shadowrocket/GoogleVoice/GoogleVoice.list`
  - 使用方式：裁剪并转换为 `domain` 行为的 `.mrs`

#### 示例结构

```text
self/
  google/
    GoogleVoice.mrs
```

------

## 🚀 使用方法

### 通用引用格式

请将 URL 替换为你需要的实际路径。

#### GitHub 原始链接

```text
https://raw.githubusercontent.com/isalikai/rules_mrs/master/<目录>/<文件名>.mrs
```

#### jsDelivr 加速（推荐国内使用）

```text
https://cdn.jsdelivr.net/gh/isalikai/rules_mrs@master/<目录>/<文件名>.mrs
```

------

## 📌 使用示例

### 示例 1：使用 `rules/` 下的 GeositeCN 规则

```yaml
rule-providers:
  geosite-cn:
    type: http
    behavior: domain
    format: mrs
    url: https://cdn.jsdelivr.net/gh/isalikai/rules_mrs@master/rules/GeositeCN/GeositeCN_Domain.mrs
    path: ./rules/GeositeCN/GeositeCN_Domain.mrs
    interval: 86400

rules:
  - RULE-SET,geosite-cn,DIRECT
```

------

### 示例 2：使用 `self/` 下的 Google Voice 自用规则

```yaml
rule-providers:
  google-voice:
    type: http
    behavior: domain
    format: mrs
    url: https://cdn.jsdelivr.net/gh/isalikai/rules_mrs@master/self/google/GoogleVoice.mrs
    path: ./ruleset/GoogleVoice.mrs
    interval: 86400

rules:
  - RULE-SET,google-voice,🇺🇸 Voice-US
```

------

## 🛠️ 转换逻辑说明

- `rules/` 目录
  - 严格按上游文件名判断规则类型
  - 不对规则内容做额外清洗或主观调整
- `self/` 目录
  - 根据规则实际用途选择最合适的行为类型
  - 允许裁剪、合并规则

------

## 🙏 致谢

- 📜 默认规则源：
  [Accademia/Additional_Rule_For_Clash](https://github.com/Accademia/Additional_Rule_For_Clash)
- 📦 自用规则参考来源：
  [blackmatrix7/ios_rule_script](https://github.com/blackmatrix7/ios_rule_script)
- 🧰 转换工具：
  [MetaCubeX/mihomo](https://github.com/MetaCubeX/mihomo)

