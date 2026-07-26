# GitHub + jsDelivr 图片仓库设计

## 目标

创建一个名为 `mcmod-images` 的公开 GitHub 仓库，用于存放 MC 中国版模组维基的长期静态图片，并通过 jsDelivr HTTPS 地址对外引用。仓库保留完整 Git 历史，本地保留一份可迁移副本。

## 约束

- GitHub 与 jsDelivr 不是无限量或永久存储服务；图片必须同时保留本地备份。
- 仓库必须公开，jsDelivr 才能直接分发。
- 不使用 Git LFS，因为 jsDelivr 不适合分发 LFS 指针文件。
- 建议单图小于 5 MB，优先使用 WebP、PNG、JPEG；避免频繁覆盖同一路径造成 CDN 旧缓存。
- 仓库尽量控制在 1 GB 内，达到该规模前迁移到对象存储。

## 仓库结构

```text
mcmod-images/
├─ images/
│  ├─ backgrounds/
│  ├─ mods/
│  ├─ entities/
│  └─ ui/
├─ docs/superpowers/specs/
├─ .gitattributes
├─ .gitignore
└─ README.md
```

目录使用小写 ASCII 名称；图片文件名采用 `page-slug-purpose-index.ext`，不使用空格和中文，以减少 URL 编码问题。

## URL 规则

开发或低风险内容可使用主分支：

```text
https://cdn.jsdelivr.net/gh/${GITHUB_USER}/mcmod-images@main/images/${CATEGORY}/${FILE}
```

正式页面优先使用发布标签，避免内容被覆盖后缓存不一致：

```text
https://cdn.jsdelivr.net/gh/${GITHUB_USER}/mcmod-images@v1.0.0/images/${CATEGORY}/${FILE}
```

图片更新时采用新文件名或新版本标签，不依赖立即刷新 CDN 缓存。

## 操作流程

1. 在本地添加或替换图片。
2. 检查扩展名、文件大小和目录位置。
3. 提交并推送至公开 GitHub 仓库。
4. 等待 jsDelivr 首次抓取，验证 HTTP 状态、内容类型和图片尺寸。
5. 将验证后的 CDN URL 写入 Wikidot 页面。

## 故障与迁移

- jsDelivr 暂时不可用时，可临时使用 GitHub Raw URL，但不将其作为长期生产地址。
- 每次新增图片都保留本地原图；定期导出仓库压缩备份。
- 若仓库接近 1 GB、流量异常或服务政策变化，迁移至腾讯云 COS/阿里云 OSS，并通过批量替换域名完成切换。

## 验证标准

- GitHub 仓库为公开状态，默认分支为 `main`。
- 目录结构、README 与仓库规则文件存在。
- 至少一个小型测试资源能够分别通过 GitHub 与 jsDelivr 返回成功响应。
- README 清楚记录 URL 模板、限制、上传与迁移方法。
