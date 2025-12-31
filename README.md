# Mihomo Rule Set (.mrs) Mirror

[![Update All Mihomo Rules](https://github.com/isalikai/rules_mrs/actions/workflows/update-rules.yml/badge.svg)](https://github.com/isalikai/rules_mrs/actions/workflows/update-rules.yml)

本项目自动镜像并转换 [Accademia/Additional_Rule_For_Clash](https://github.com/Accademia/Additional_Rule_For_Clash) 仓库中的所有规则为 Mihomo `.mrs` 二进制格式。

✅ **保持原仓库目录结构** ✅ **每天自动更新** ✅ **包含 Domain 和 IP 规则**

## 📂 规则目录结构

所有规则均位于 `rules/` 目录下，子文件夹结构与上游仓库一致。

例如：
- 上游: `GeositeCN/GeositeCN_Domain.yaml`
- 本地: `rules/GeositeCN/GeositeCN_Domain.mrs`

## 🚀 使用方法

### 通用引用格式

请将 URL 替换为你需要的文件路径：

**GitHub 原始链接**:
`https://raw.githubusercontent.com/isalikai/rules_mrs/master/rules/<分类>/<文件名>.mrs`

**jsDelivr 加速链接 (推荐国内使用)**:
`https://cdn.jsdelivr.net/gh/isalikai/rules_mrs@master/rules/<分类>/<文件名>.mrs`

### 示例：使用 GeositeCN 规则

**config.yaml 配置:**

```yaml
rule-providers:
  geosite-cn:
    type: http
    behavior: domain
    format: mrs
    # 注意路径对应 rules/GeositeCN/...
    url: "[https://cdn.jsdelivr.net/gh/isalikai/rules_mrs@master/rules/GeositeCN/GeositeCN_Domain.mrs](https://cdn.jsdelivr.net/gh/isalikai/rules_mrs@master/rules/GeositeCN/GeositeCN_Domain.mrs)"
    path: ./rules/GeositeCN/GeositeCN_Domain.mrs
    interval: 86400

rules:
  - RULE-SET,geosite-cn,DIRECT

```

## 🛠️ 转换逻辑

脚本会自动根据文件名判断规则类型：

* 文件名包含 `IP` 或 `ip` -> 转换为 `ipcidr` 类型
* 其他 -> 默认为 `domain` 类型

## 🙏 致谢

* 规则源: [Accademia/Additional_Rule_For_Clash](https://github.com/Accademia/Additional_Rule_For_Clash)
* 工具: [MetaCubeX/mihomo](https://github.com/MetaCubeX/mihomo)
