# Bark Action

[![GitHub Marketplace](https://img.shields.io/badge/Marketplace-Bark%20Notification-blue.svg?colorA=24292e&colorB=0366d6&style=flat&longCache=true&logo=data:images/png;base64,iVBORw0KGgoAAAANSUhEUgAAAA4AAAAOCAYAAAAfSC3RAAAABHNCSVQICAgIfAhkiAAAAAlwSFlzAAAM6wAADOsB5dZE0gAAABl0RVh0U29mdHdhcmUAd3d3Lmlua3NjYXBlLm9yZ5vuPBoAAAERSURBVCiRhZG/SsMxFEZPfsVJ61jbxaF0cRQRcRJ9hlYn30IHN/+9iquDCOIsblIrOjqKgy5aKoJQj4O3EEtbPwhJbr6Te28CmdSKeqzeqr0YbfVIrTBKakvtOl5dtTkK+v4HfA9PEyBFCY9AGVgCBLaBp1jPAyfAJ/AAdIEG0dNAiyP7+K1qIfMdonZic6+WJoBJvQlvuwDqcXadUuqPA1NKAlexbRTAIMvMOCjTbMwl1LtI/6KWJ5Q6rT6Ht1MA58AX8Apcqqt5r2qhrgAXQC3CZ6i1+KMd9TRu3MvA3aH/fFPnBodb6oe6HM8+lYHrGdRXW8M9bMZtPXUji69lmf5Cmamq7quNLFZXD9Rq7v0Bpc1o/tp0fisAAAAASUVORK5CYII=)](https://github.com/marketplace/actions/bark-notification)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

A GitHub Action that sends push notifications via [Bark](https://github.com/Finb/Bark) with customizable title and content format.

[中文文档](README_zh.md)

## Features

- 📱 Send push notifications to iOS devices via Bark
- 🎨 Customizable notification title and body
- 🔗 Support for URL redirection
- 🔔 Multiple notification sounds
- 📋 Automatic clipboard copy
- 📦 Archive notifications to history
- 🎯 Support for custom Bark server

## Usage

### Basic Usage

```yaml
- name: Send Bark Notification
  uses: lihuacai168/bark-action@v1
  with:
    key: ${{ secrets.BARK_KEY }}
    title: 'Build Success'
    body: 'Your build has completed successfully!'
    url: ${{ github.server_url }}/${{ github.repository }}/actions/runs/${{ github.run_id }}
```

### Complete Example

```yaml
name: CI with Bark Notification

on: [push, pull_request]

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout code
        uses: actions/checkout@v4
      
      - name: Build project
        run: |
          # Your build commands here
          echo "Building..."
      
      - name: Send success notification
        if: success()
        uses: lihuacai168/bark-action@v1
        with:
          key: ${{ secrets.BARK_KEY }}
          title: '${{ github.repository }} - ${{ github.workflow }} - Success'
          body: 'Build completed successfully'
          url: ${{ github.server_url }}/${{ github.repository }}/actions/runs/${{ github.run_id }}
          sound: 'bell'
          isArchive: '1'
      
      - name: Send failure notification
        if: failure()
        uses: lihuacai168/bark-action@v1
        with:
          key: ${{ secrets.BARK_KEY }}
          title: '${{ github.repository }} - ${{ github.workflow }} - Failed'
          body: 'Build failed, please check the logs'
          url: ${{ github.server_url }}/${{ github.repository }}/actions/runs/${{ github.run_id }}
          sound: 'alarm'
          isArchive: '1'
```

## Inputs

| Input | Description | Required | Default |
|-------|-------------|----------|---------|
| `key` | Bark API Key | ✅ Yes | - |
| `host` | Bark server host | No | `https://api.day.app` |
| `title` | Notification title | No | `GitHub Actions Notification` |
| `body` | Notification body | No | `Workflow completed` |
| `url` | URL to open when tapped | No | - |
| `sound` | Notification sound | No | - |
| `isArchive` | Save to history (1 for yes) | No | - |
| `automaticallyCopy` | Auto copy to clipboard (1 for yes) | No | - |
| `copy` | Content to copy | No | - |
| `group` | Notification group | No | - |
| `icon` | Notification icon URL | No | - |

### Available Sounds

- `bell` - Bell sound
- `alarm` - Alarm sound
- `anticipate` - Anticipate
- `bloom` - Bloom
- `calypso` - Calypso
- `chime` - Chime
- `choo` - Choo
- `descent` - Descent
- `electronic` - Electronic
- `fanfare` - Fanfare
- `glass` - Glass
- `gotosleep` - Go to sleep
- `healthnotification` - Health notification
- `horn` - Horn
- `ladder` - Ladder
- `mailsent` - Mail sent
- `minuet` - Minuet
- `multiwayinvitation` - Multiway invitation
- `newmail` - New mail
- `newsflash` - News flash
- `noir` - Noir
- `paymentsuccess` - Payment success
- `shake` - Shake
- `sherwoodforest` - Sherwood forest
- `silence` - Silence
- `spell` - Spell
- `suspense` - Suspense
- `telegraph` - Telegraph
- `tiptoes` - Tiptoes
- `typewriters` - Typewriters
- `update` - Update

## Setup

### 1. Install Bark App

Download Bark from App Store (iOS only):
- [Bark on App Store](https://apps.apple.com/cn/app/bark/id1403753865)

### 2. Get Your Bark Key

Open the Bark app and copy your device key from the URL shown in the app.

Example: If your URL is `https://api.day.app/YOUR_BARK_KEY_HERE`, then `YOUR_BARK_KEY_HERE` is your key.

### 3. Add Secret to GitHub

1. Go to your GitHub repository
2. Navigate to `Settings` → `Secrets and variables` → `Actions`
3. Click `New repository secret`
4. Name: `BARK_KEY`
5. Value: Your Bark key
6. Click `Add secret`

Or use GitHub CLI:

```bash
gh secret set BARK_KEY --body "your_bark_key_here"
```

## Examples

### Notify on Success and Failure

```yaml
- name: Send notification
  if: always()
  uses: lihuacai168/bark-action@v1
  with:
    key: ${{ secrets.BARK_KEY }}
    title: '${{ github.repository }} - ${{ job.status }}'
    body: 'Job ${{ github.job }} ${{ job.status }}'
    url: ${{ github.server_url }}/${{ github.repository }}/actions/runs/${{ github.run_id }}
    sound: ${{ job.status == 'success' && 'bell' || 'alarm' }}
```

### Copy Build Version to Clipboard

```yaml
- name: Send notification with version
  uses: lihuacai168/bark-action@v1
  with:
    key: ${{ secrets.BARK_KEY }}
    title: 'Release Published'
    body: 'Version ${{ github.ref_name }} is now available'
    copy: ${{ github.ref_name }}
    automaticallyCopy: '1'
```

### Group Notifications

```yaml
- name: Send grouped notification
  uses: lihuacai168/bark-action@v1
  with:
    key: ${{ secrets.BARK_KEY }}
    title: 'Build Status'
    body: '${{ github.workflow }} completed'
    group: ${{ github.repository }}
```

## License

MIT License - see the [LICENSE](LICENSE) file for details.

## Credits

Inspired by [shink/bark-action](https://github.com/shink/bark-action) and powered by [Finb/Bark](https://github.com/Finb/Bark).

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

