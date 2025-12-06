# Bark Action

[![GitHub Marketplace](https://img.shields.io/badge/Marketplace-Bark%20Notification-blue.svg?colorA=24292e&colorB=0366d6&style=flat&longCache=true&logo=data:images/png;base64,iVBORw0KGgoAAAANSUhEUgAAAA4AAAAOCAYAAAAfSC3RAAAABHNCSVQICAgIfAhkiAAAAAlwSFlzAAAM6wAADOsB5dZE0gAAABl0RVh0U29mdHdhcmUAd3d3Lmlua3NjYXBlLm9yZ5vuPBoAAAERSURBVCiRhZG/SsMxFEZPfsVJ61jbxaF0cRQRcRJ9hlYn30IHN/+9iquDCOIsblIrOjqKgy5aKoJQj4O3EEtbPwhJbr6Te28CmdSKeqzeqr0YbfVIrTBKakvtOl5dtTkK+v4HfA9PEyBFCY9AGVgCBLaBp1jPAyfAJ/AAdIEG0dNAiyP7+K1qIfMdonZic6+WJoBJvQlvuwDqcXadUuqPA1NKAlexbRTAIMvMOCjTbMwl1LtI/6KWJ5Q6rT6Ht1MA58AX8Apcqqt5r2qhrgAXQC3CZ6i1+KMd9TRu3MvA3aH/fFPnBodb6oe6HM8+lYHrGdRXW8M9bMZtPXUji69lmf5Cmamq7quNLFZXD9Rq7v0Bpc1o/tp0fisAAAAASUVORK5CYII=)](https://github.com/marketplace/actions/bark-notification)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

一个通过 [Bark](https://github.com/Finb/Bark) 发送推送通知的 GitHub Action，支持自定义标题和内容格式。

[English Documentation](README.md)

## 特性

- 📱 通过 Bark 向 iOS 设备发送推送通知
- 🎨 可自定义通知标题和内容
- 🔗 支持 URL 跳转
- 🔔 多种通知提示音
- 📋 自动复制到剪贴板
- 📦 归档通知到历史记录
- 🎯 支持自定义 Bark 服务器

## 使用方法

### 基础用法

```yaml
- name: 发送 Bark 通知
  uses: lihuacai168/bark-action@v1
  with:
    key: ${{ secrets.BARK_KEY }}
    title: '构建成功'
    body: '您的构建已成功完成！'
    url: ${{ github.server_url }}/${{ github.repository }}/actions/runs/${{ github.run_id }}
```

### 完整示例

```yaml
name: 带 Bark 通知的 CI

on: [push, pull_request]

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: 检出代码
        uses: actions/checkout@v4
      
      - name: 构建项目
        run: |
          # 你的构建命令
          echo "Building..."
      
      - name: 发送成功通知
        if: success()
        uses: lihuacai168/bark-action@v1
        with:
          key: ${{ secrets.BARK_KEY }}
          title: '${{ github.repository }} - ${{ github.workflow }} - 成功'
          body: '构建成功完成'
          url: ${{ github.server_url }}/${{ github.repository }}/actions/runs/${{ github.run_id }}
          sound: 'bell'
          isArchive: '1'
      
      - name: 发送失败通知
        if: failure()
        uses: lihuacai168/bark-action@v1
        with:
          key: ${{ secrets.BARK_KEY }}
          title: '${{ github.repository }} - ${{ github.workflow }} - 失败'
          body: '构建失败，请检查日志'
          url: ${{ github.server_url }}/${{ github.repository }}/actions/runs/${{ github.run_id }}
          sound: 'alarm'
          isArchive: '1'
```

## 输入参数

| 参数 | 描述 | 必需 | 默认值 |
|------|------|------|--------|
| `key` | Bark API Key | ✅ 是 | - |
| `host` | Bark 服务器地址 | 否 | `https://api.day.app` |
| `title` | 通知标题 | 否 | `GitHub Actions Notification` |
| `body` | 通知内容 | 否 | `Workflow completed` |
| `url` | 点击通知时打开的 URL | 否 | - |
| `sound` | 通知提示音 | 否 | - |
| `isArchive` | 是否保存到历史记录（1 表示是） | 否 | - |
| `automaticallyCopy` | 自动复制到剪贴板（1 表示是） | 否 | - |
| `copy` | 要复制的内容 | 否 | - |
| `group` | 通知分组 | 否 | - |
| `icon` | 通知图标 URL | 否 | - |

### 可用的提示音

- `bell` - 铃声
- `alarm` - 闹钟
- `anticipate` - 期待
- `bloom` - 绽放
- `calypso` - 卡里普索
- `chime` - 钟声
- `choo` - 火车
- `descent` - 下降
- `electronic` - 电子
- `fanfare` - 号角
- `glass` - 玻璃
- `gotosleep` - 睡觉
- `healthnotification` - 健康通知
- `horn` - 喇叭
- `ladder` - 梯子
- `mailsent` - 邮件已发送
- `minuet` - 小步舞曲
- `multiwayinvitation` - 多人邀请
- `newmail` - 新邮件
- `newsflash` - 新闻快讯
- `noir` - 黑色电影
- `paymentsuccess` - 支付成功
- `shake` - 摇动
- `sherwoodforest` - 舍伍德森林
- `silence` - 静音
- `spell` - 咒语
- `suspense` - 悬念
- `telegraph` - 电报
- `tiptoes` - 蹑手蹑脚
- `typewriters` - 打字机
- `update` - 更新

## 设置步骤

### 1. 安装 Bark 应用

从 App Store 下载 Bark（仅 iOS）：
- [App Store 上的 Bark](https://apps.apple.com/cn/app/bark/id1403753865)

### 2. 获取您的 Bark Key

打开 Bark 应用，从应用中显示的 URL 复制您的设备密钥。

例如：如果您的 URL 是 `https://api.day.app/YOUR_BARK_KEY_HERE`，那么 `YOUR_BARK_KEY_HERE` 就是您的 key。

### 3. 添加 Secret 到 GitHub

1. 进入您的 GitHub 仓库
2. 导航至 `Settings` → `Secrets and variables` → `Actions`
3. 点击 `New repository secret`
4. 名称：`BARK_KEY`
5. 值：您的 Bark key
6. 点击 `Add secret`

或使用 GitHub CLI：

```bash
gh secret set BARK_KEY --body "your_bark_key_here"
```

## 示例

### 成功和失败时都发送通知

```yaml
- name: 发送通知
  if: always()
  uses: lihuacai168/bark-action@v1
  with:
    key: ${{ secrets.BARK_KEY }}
    title: '${{ github.repository }} - ${{ job.status }}'
    body: '任务 ${{ github.job }} ${{ job.status }}'
    url: ${{ github.server_url }}/${{ github.repository }}/actions/runs/${{ github.run_id }}
    sound: ${{ job.status == 'success' && 'bell' || 'alarm' }}
```

### 将构建版本复制到剪贴板

```yaml
- name: 发送带版本号的通知
  uses: lihuacai168/bark-action@v1
  with:
    key: ${{ secrets.BARK_KEY }}
    title: '发布已完成'
    body: '版本 ${{ github.ref_name }} 现已可用'
    copy: ${{ github.ref_name }}
    automaticallyCopy: '1'
```

### 分组通知

```yaml
- name: 发送分组通知
  uses: lihuacai168/bark-action@v1
  with:
    key: ${{ secrets.BARK_KEY }}
    title: '构建状态'
    body: '${{ github.workflow }} 已完成'
    group: ${{ github.repository }}
```

## 许可证

MIT License - 详见 [LICENSE](LICENSE) 文件。

## 致谢

灵感来自 [shink/bark-action](https://github.com/shink/bark-action)，基于 [Finb/Bark](https://github.com/Finb/Bark) 实现。

## 贡献

欢迎贡献！请随时提交 Pull Request。

