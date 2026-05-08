# Mihomo Rule Set (.mrs) Mirror

[![Update All Mihomo Rules](https://github.com/isalikai/rules_mrs/actions/workflows/update-rules.yml/badge.svg)](https://github.com/isalikai/rules_mrs/actions/workflows/update-rules.yml)

本项目用于 **自动同步、转换并维护 Mihomo 可用的 `.mrs` 规则集**。

规则来源为公开上游仓库，项目通过 GitHub Actions 定时拉取上游规则，并将适合转换的 Domain / IP 规则转换为 Mihomo 支持的 `.mrs` 二进制规则集。

所有规则均通过 GitHub Actions **每日自动更新**。

---

## ✨ 项目特性

- ✅ 自动同步上游规则仓库
- ✅ 自动转换为 Mihomo `.mrs` 二进制格式
- ✅ 保持上游原始目录结构
- ✅ 支持 Domain / IP 规则集
- ✅ 不对上游规则内容做主观修改
- ✅ 适合在 Mihomo / Clash Meta 配置中作为 rule-provider 引用

---

## 📂 目录结构说明

### `rules/` —— 上游规则镜像

`rules/` 目录用于 **镜像并转换** 以下上游仓库中的规则：

- 📦 **规则源**：  
  [Accademia/Additional_Rule_For_Clash](https://github.com/Accademia/Additional_Rule_For_Clash)

转换后的规则会保留上游仓库中的目录结构。

特点：

- ✔ 与上游仓库目录结构保持一致
- ✔ 仅转换适合 Mihomo `.mrs` 的规则文件
- ✔ 仅支持 `domain` 与 `ipcidr` 两类 behavior
- ✔ 不对规则内容做额外清洗或主观调整
- ✔ 适合公开、通用引用

---

## 🔄 转换规则说明

本项目仅转换上游仓库中明确的 Domain / IP 规则文件：

```text
*_Domain.yaml -> behavior: domain  -> .mrs
*_IP.yaml     -> behavior: ipcidr  -> .mrs
```

不会转换普通混合规则、classical 规则或其他无法明确归类为 Domain / IP 的规则。

原因是 Mihomo 的 `.mrs` 规则集主要适用于：

```text
domain
ipcidr
```

如果一个规则文件中混合了多种规则类型，例如：

```text
DOMAIN
IP-CIDR
PROCESS-NAME
GEOIP
MATCH
```

则不适合直接转换为单个 `.mrs` 文件。

---

## 📌 文件示例

| 上游文件                          | 本仓库生成文件                         | Behavior |
| --------------------------------- | -------------------------------------- | -------- |
| `GeositeCN/GeositeCN_Domain.yaml` | `rules/GeositeCN/GeositeCN_Domain.mrs` | domain   |
| `Google/Google_IP.yaml`           | `rules/Google/Google_IP.mrs`           | ipcidr   |

---

## 🚀 使用方法

### GitHub 原始链接

```text
https://raw.githubusercontent.com/isalikai/rules_mrs/master/<目录>/<文件名>.mrs
```

### jsDelivr 加速链接

```text
https://cdn.jsdelivr.net/gh/isalikai/rules_mrs@master/<目录>/<文件名>.mrs
```

示例：

```text
https://cdn.jsdelivr.net/gh/isalikai/rules_mrs@master/rules/GeositeCN/GeositeCN_Domain.mrs
```

---

## 📌 Mihomo 配置示例

### 示例 1：引用 Domain 类型规则

```yaml
rule-providers:
  geosite-cn:
    type: http
    behavior: domain
    format: mrs
    url: https://cdn.jsdelivr.net/gh/isalikai/rules_mrs@master/rules/GeositeCN/GeositeCN_Domain.mrs
    path: ./ruleset/GeositeCN_Domain.mrs
    interval: 86400

rules:
  - RULE-SET,geosite-cn,DIRECT
```

---

### 示例 2：引用 IP 类型规则

```yaml
rule-providers:
  google-ip:
    type: http
    behavior: ipcidr
    format: mrs
    url: https://cdn.jsdelivr.net/gh/isalikai/rules_mrs@master/rules/Google/Google_IP.mrs
    path: ./ruleset/Google_IP.mrs
    interval: 86400

rules:
  - RULE-SET,google-ip,Google
```

> 注意：`RULE-SET` 后面的策略组名称需要替换为你自己配置中真实存在的策略组名称。

---

## ⚠️ 注意事项

### 1. `behavior` 必须和规则类型一致

Domain 规则：

```yaml
behavior: domain
format: mrs
```

IP 规则：

```yaml
behavior: ipcidr
format: mrs
```

如果 `behavior` 写错，规则可能无法正常生效。

---

### 2. `.mrs` 不是通用规则格式

`.mrs` 不适合直接保存所有 Clash / Mihomo 规则类型。

本项目主要处理：

```text
DOMAIN
DOMAIN-SUFFIX
DOMAIN-KEYWORD
IP-CIDR
IP-CIDR6
```

不处理或不保证支持：

```text
PROCESS-NAME
USER-AGENT
GEOIP
MATCH
SCRIPT
classical 混合规则
```

---

### 3. 规则顺序很重要

Mihomo 会按照 `rules:` 中的顺序从上到下匹配。

如果某个规则需要优先生效，应放在更大的规则集前面。

例如：

```yaml
rules:
  - RULE-SET,gemini-domain,AI
  - RULE-SET,google-domain,Google
  - MATCH,Final
```

如果 `google-domain` 放在 `gemini-domain` 前面，部分流量可能会提前被 Google 规则匹配走。

---

### 4. 更新后需要刷新 rule-provider

仓库中的 `.mrs` 更新后，客户端不一定会立刻使用新规则。

如果发现规则没有变化，可以在 Mihomo 面板中手动更新 rule-provider，或者删除本地缓存后重启 Mihomo。

---

## 🛠️ 自动更新说明

本项目通过 GitHub Actions 自动执行以下流程：

```text
1. 拉取最新 Mihomo 程序
2. 克隆上游规则仓库
3. 清理旧的 rules/ 目录
4. 转换 *_Domain.yaml 为 domain .mrs
5. 转换 *_IP.yaml 为 ipcidr .mrs
6. 提交并推送更新后的规则文件
```

定时任务默认每日运行一次：

```yaml
cron: '0 22 * * *'
```

对应北京时间约为每日 06:00。

---

## 🙏 致谢

- 📜 规则来源：  
  [Accademia/Additional_Rule_For_Clash](https://github.com/Accademia/Additional_Rule_For_Clash)

- 🧰 转换工具：  
  [MetaCubeX/mihomo](https://github.com/MetaCubeX/mihomo)
