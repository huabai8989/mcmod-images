# mcmod-images

用于 MC 中国版模组维基的公开静态图片资源仓库。

## 直链示例

`https://cdn.jsdelivr.net/gh/huabai8989/mcmod-images@main/images/ui/jsdelivr-check.svg`

## 目录说明

- `images/backgrounds/`：网站及页面背景图
- `images/mods/`：模组海报和截图
- `images/entities/`：实体插图和截图
- `images/ui/`：图标及界面图片

## 使用规则

- 文件名使用小写英文字母、数字和连字符，不要包含空格或中文。
- 优先使用 WebP、PNG 或 JPEG 格式，单张图片建议小于 5 MB。
- 不要使用 Git LFS，否则 jsDelivr 无法直接提供图片文件。
- 不要覆盖已经发布的图片路径；更新图片时请在文件名末尾添加版本号。
- 请在 GitHub 之外保留原始图片备份。

## 获取图片直链

将图片提交到 `main` 分支后，按照下面的格式生成 jsDelivr 地址：

```text
https://cdn.jsdelivr.net/gh/huabai8989/mcmod-images@main/图片在仓库中的路径
```

例如，仓库文件 `images/ui/jsdelivr-check.svg` 对应：

```text
https://cdn.jsdelivr.net/gh/huabai8989/mcmod-images@main/images/ui/jsdelivr-check.svg
```
