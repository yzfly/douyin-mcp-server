---
name: douyin-video
description: "抖音无水印视频下载和文案提取工具. 从抖音分享链接获取无水印视频下载链接, 下载视频, 提取视频中的语音文案并自动保存到文件. 适用场景包括获取抖音视频信息, 下载无水印视频, 批量提取视频文案. 当用户需要处理抖音视频链接或提取视频内容时触发."
---

# 抖音无水印视频下载和文案提取

从抖音分享链接获取无水印视频下载链接, 下载视频, 并使用语音识别提取视频中的文案, 自动保存到文件.

## 功能概述

- **获取下载链接**: 从抖音分享链接解析出无水印视频的直接下载地址 (无需 API 密钥)
- **下载视频**: 将无水印视频下载到本地指定目录
- **提取文案**: 通过语音识别从视频中提取文字内容 (需要硅基流动 API 密钥)
- **自动保存**: 每个视频的文案自动保存到独立文件夹 (视频ID为文件夹名)

## 环境要求

### 依赖安装

```bash
pip install requests ffmpeg-python
```

### 系统要求

- FFmpeg 必须安装在系统中 (用于音视频处理)
- macOS: `brew install ffmpeg`
- Ubuntu: `apt install ffmpeg`

### API 密钥配置 (仅文案提取需要)

文案提取功能使用硅基流动 API, 需要设置环境变量:

```bash
export API_KEY="your-siliconflow-api-key"
```

获取 API 密钥: https://cloud.siliconflow.cn/

## 使用方法

### 方法一: 使用脚本 (推荐)

```bash
# 获取视频信息和下载链接 (无需 API 密钥)
python douyin_downloader.py --link "抖音分享链接" --action info

# 下载视频到指定目录
python douyin_downloader.py --link "抖音分享链接" --action download --output ./videos

# 提取视频文案并保存到文件 (需要 API_KEY 环境变量)
python douyin_downloader.py --link "抖音分享链接" --action extract --output ./output

# 提取文案并同时保存视频
python douyin_downloader.py --link "抖音分享链接" --action extract --output ./output --save-video

# 安静模式 (减少输出)
python douyin_downloader.py --link "抖音分享链接" --action extract --output ./output --quiet
```

### 输出目录结构

提取文案后, 每个视频会保存到独立文件夹:

```
output/
├── 7600361826030865707/      # 视频ID为文件夹名
│   └── transcript.md         # Markdown 格式文案文件
├── 7581044356631612699/
│   ├── transcript.md
│   └── 7581044356631612699.mp4  # 使用 --save-video 时保存
└── ...
```

### Markdown 文案格式

```markdown
# 视频标题

| 属性 | 值 |
|------|-----|
| 视频ID | `7600361826030865707` |
| 提取时间 | 2026-01-30 14:19:00 |
| 下载链接 | [点击下载](url) |

---

## 文案内容

(语音识别的文字内容)
```

### 方法二: 在 Python 代码中调用

```python
from douyin_downloader import get_video_info, download_video, extract_text

# 获取视频信息
info = get_video_info("抖音分享链接")
print(f"视频ID: {info['video_id']}")
print(f"标题: {info['title']}")
print(f"下载链接: {info['url']}")

# 下载视频
video_path = download_video("抖音分享链接", output_dir="./videos")

# 提取文案并保存到文件
result = extract_text("抖音分享链接", output_dir="./output")
print(f"文案已保存到: {result['output_path']}")
print(result['text'])
```

## 工作流程

### 获取视频信息

1. 解析抖音分享链接, 提取真实的视频 URL
2. 模拟移动端请求获取页面数据
3. 从页面 JSON 数据中提取无水印视频地址
4. 返回视频 ID, 标题和下载链接

### 提取视频文案

1. 解析分享链接获取视频信息
2. 下载视频到临时目录
3. 使用 FFmpeg 从视频中提取音频 (MP3 格式)
4. 调用硅基流动 SenseVoice API 进行语音识别
5. 清理临时文件, 返回识别的文本

## 常见问题

### 无法解析链接

- 确保链接是有效的抖音分享链接
- 链接格式通常为 `https://v.douyin.com/xxxxx/` 或完整的抖音视频 URL

### 提取文案失败

- 检查 `API_KEY` 环境变量是否已设置
- 确保 API 密钥有效且有足够的配额
- 确保 FFmpeg 已正确安装

### 下载速度慢

- 这取决于网络条件和视频大小
- 脚本会显示下载进度

## 注意事项

- 本工具仅供学习和研究使用
- 使用时需遵守相关法律法规
- 请勿用于任何侵犯版权或违法的目的
