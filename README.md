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
- 使用上游 JavaScript 对菜鸟首页、弹窗和推广内容的 HTTP 响应做清理。开屏广告池的 `nbnetflow.ads.*` 现在统一通过 URL Rewrite 直接拦截。
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

## 更新记录

- **v1.1（2026-11-14）**：修正 `nbnetflow.ads.*` 广告接口拦截。所有同系列接口统一使用 `reject-dict`，不再让其中的 show/mshow/index 依赖上游 JS；避免脚本与拒绝规则相互争用。此变更不能保证清除其他广告 SDK 或本地缓存中的开屏视频。

## 开屏仍有广告：如何排查

1. 在 Shadowrocket 的 **配置 → 模块** 中刷新/更新此远程模块，检查描述含 `v1.1`，保持模块勾选启用。
2. 在当前配置的 **HTTPS 解密** 中确认开关已启用、CA 证书已被系统完全信任；设置页面只显示主配置原有域名，不一定列出模块追加的主机名。
3. 在 **数据/请求日志** 中搜索 `mtop.cainiao`、`guide-acs.m.taobao.com`、`cn-acs.m.cainiao.com`，检查是否能查看完整 HTTPS URL 和响应，而不只是 CONNECT 或域名；仅仅看到第三方 SDK 的 `REJECT` 不足以证明 MTop 内容清洗已生效。
4. 若应用内提供“清理缓存”，先尝试该功能；强制退出后重新进入再测试。**不要为了测试直接卸载 App**，以免丢失本地数据。
5. 仍存在视频开屏时，在广告出现前后清空并导出**已脱敏**的请求日志/HAR；明确说明是启动广告还是首页广告。请勿公开包含手机号、收货地址、Cookie、Token、session、物流信息等敏感字段的原始抓包。

> 开屏视频可能来自独立广告 SDK、预加载资源或菜鸟主网关。仅凭第三方 SDK 的统计域名被拒绝，不能确定广告素材实际来源；需要相应请求才能安全追加规则。默认不大范围阻断抖音或阿里共享 CDN 域名，以免影响其他 App。

- **v1.2（2026-11-14）**：根据用户提供的 Shadowrocket SQLite 日志（17:43:33–17:46:13，共 203 条请求）补充腾讯优量汇 `gdt.qq.com` 子域、`sdk.e.qq.com`、`sdkquic.e.qq.com` 和穿山甲 `pangolin-sdk-toutiao1.com`、`pangolin-sdk-toutiao-b.com` 的域名拦截。原日志显示这些域名曾被 DIRECT 放行；这些规则针对整个设备生效，而不是仅菜鸟 App，可能影响其他 App 中的广告与 SDK。没有拦截 `qq.com` 全域、`bytescm.com`、`douyinvod.com` 等共享服务。

> **证据边界**：本次 SQLite 数据库包含请求 URL、分流结果及时间，不含完整 HTTP 响应体，也未记录广告素材与视频的对应关系。命中广告 SDK 并不等于确认开屏视频的实际来源。重新启动 App 后若视频仍在，请提供广告出现前后的**脱敏**网络请求记录，或在应用内安全清理缓存再测试。

- **v1.3（2026-11-14）**：新日志 `proxy-2026-11-14-174923.db` 共 746 条请求，17:51:02 起可确认 v1.2 广告域名规则命中；`netflow-mtop.cainiao.com` 的 `mshow/index/batch.show` 系列共 14 条请求被 `REJECT-DICT` 拒绝。新增精确域名 `adsmind.ugdtimg.com`（17:51:20 两次 DIRECT），避免拦截共享 CDN `lf-cdn-tos.bytescm.com`。SQLite 中 `DIRECT` 表示分流结果，不能据此判断 HTTP 响应清理脚本是否成功。
