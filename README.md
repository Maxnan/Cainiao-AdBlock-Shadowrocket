# 菜鸟去广告 · Shadowrocket 整合模块

适用于 **Shadowrocket（小火箭）** 的菜鸟 App 广告处理模块（`.sgmodule`)。

该模块基于以下两个项目的公开规则思路进行整合，并非其官方发布：

- [UserC23/cainiao-ads-loon](https://github.com/UserC23/cainiao-ads-loon)（Loon）
- [JEFFRY-ZH/Cainiao-AdBlock](https://github.com/JEFFRY-ZH/Cainiao-AdBlock)（Quantumult X 脚本）

## 模块地址

仓库创建并上传后，可将以下 **Raw 直链** 添加为 Shadowrocket 的远程模块：

```text
https://raw.githubusercontent.com/Maxnan/Cainiao-AdBlock-Shadowrocket/main/Cainiao-AdBlock-Shadowrocket.sgmodule
```

> 上述链接需要仓库是公开的，且文件已上传到 `main` 分支后才能访问。

## 功能

- 拦截部分广告 SDK、素材和广告监测域名。
- URL 重写拒绝部分独立广告接口。
- 使用上游 JavaScript 对菜鸟首页、弹窗、广告池和推广内容的 HTTP 响应做清理。
- 保留查快递、取件、寄件等核心功能为优先目标，但**不能保证各版本完全兼容**。

## 使用方法

1. 在 Shadowrocket 中进入 **配置 → 模块**，从 URL 添加上述 `.sgmodule` 的 Raw 直链，或使用本地文件导入。
2. 启用模块，并根据 Shadowrocket 的 HTTPS 解密设置安装、信任 CA 证书。
3. 关闭菜鸟 App 后重新打开，检查广告是否被清理及核心功能是否正常。
4. 更新模块时，在 Shadowrocket 中刷新远程模块以获取最新规则。

> **提示**：完整响应清理依赖 HTTPS 解密；只导入模块而不启用解密可能仅有部分域名规则生效。

## 注意事项

- 规则中的第三方域名阻断作用于**整个设备**，可能影响其他 App；可按需注释相关规则。
- HTTPS 解密配置包括部分淘宝共享 MTop 网关，可能覆盖其他阿里系 App 的部分流量；建议仅在理解风险后启用。
- JavaScript 脚本目前通过上游 GitHub Raw URL 加载，**不托管在本仓库**。上游若更新、移动或删除脚本，可能影响去广告效果。
- 不要与其他菜鸟去广告模块同时开启，以免出现重写或脚本执行冲突。
- 此模块未在所有菜鸟 App 版本、所有 iOS 和 Shadowrocket 版本中进行验证，使用前请备份当前配置。

## 文件说明

| 文件 | 说明 |
| --- | --- |
| `Cainiao-AdBlock-Shadowrocket.sgmodule` | 可直接导入或远程订阅的小火箭模块 |
| `README.md` | 项目说明与安装方法 |

## 鸣谢

感谢 [UserC23](https://github.com/UserC23/cainiao-ads-loon) 和 [JEFFRY-ZH](https://github.com/JEFFRY-ZH/Cainiao-AdBlock) 提供的原始项目。原始代码和规则的许可条款以各上游仓库为准。

## 维护

发现误拦截或残留广告，可在仓库 Issue 中说明：菜鸟版本、iOS/Shadowrocket 版本、失效页面（注意脱敏），以便后续调整。
