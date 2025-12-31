# Mihomo Rule Set (.mrs) Auto-Converter 

本项目通过 GitHub Actions 每天自动抓取上游优秀的规则列表，并调用 **Mihomo (Clash Meta)** 内核将其编译为二进制的 `.mrs` (Mihomo Rule Set) 格式。

`.mrs` 格式相比传统的 `.yaml` 或 `.txt` 规则集，具有**加载速度更快**、**内存占用更低**的优势，特别适合在性能有限的软路由或移动设备上使用。

## 📅 更新策略

- **更新频率**：每天北京时间早上 06:00 自动运行
- **上游源**：[Accademia/Additional_Rule_For_Clash](https://github.com/Accademia/Additional_Rule_For_Clash)
- **目标文件**：`GeositeCN_Domain.yaml` (转换为 `geosite_cn.mrs`)

## 🚀 规则订阅地址

| 规则名称 | 原始格式 | 编译格式 | 原始链接 (GitHub) | 加速链接 (jsDelivr) |
| :--- | :---: | :---: | :--- | :--- |
| **Geosite CN** | YAML | **MRS** | [点击复制](https://raw.githubusercontent.com/isalikai/rules_mrs/main/rules/geosite_cn.mrs) | [点击复制](https://cdn.jsdelivr.net/gh/isalikai/rules_mrs@main/rules/geosite_cn.mrs) |

> **提示**：如果在国内网络环境使用，建议使用 **加速链接 (jsDelivr)**。

## 🛠️ 如何在 Mihomo (Clash Meta) 中使用

在你的配置文件（通常是 `config.yaml`）中，按照以下方式添加 `rule-providers` 和 `rules`。

### 1. 添加 Rule Provider

注意 `format` 必须设置为 `mrs`。

```yaml
rule-providers:
  geosite-cn:
    type: http
    behavior: domain
    format: mrs           # 关键：指定格式为 mrs
    path: ./rules/geosite_cn.mrs
    url: "[https://cdn.jsdelivr.net/gh/](https://cdn.jsdelivr.net/gh/)isalikai/rules_mrs@main/rules/geosite_cn.mrs"
    interval: 86400       # 每天更新一次

```

### 2. 应用规则

在 `rules` 部分引用上面定义的 provider。

```yaml
rules:
  # 将 geosite-cn 中的域名直连
  - RULE-SET,geosite-cn,DIRECT
  
  # ... 其他规则 ...
  - MATCH,PROXY

```

## ⚙️ 编译原理

本项目使用 GitHub Actions 自动化流程：

1. 下载最新的 Mihomo Kernel。
2. 下载上游 YAML 规则文件。
3. 执行 `mihomo convert-ruleset domain yaml input.yaml output.mrs` 命令。
4. 将生成的 `.mrs` 文件推送到本仓库。

## 🙏 致谢

* 规则来源：[Accademia/Additional_Rule_For_Clash](https://github.com/Accademia/Additional_Rule_For_Clash)
* 编译工具：[MetaCubeX/mihomo](https://github.com/MetaCubeX/mihomo)

