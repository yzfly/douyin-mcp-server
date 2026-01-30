# 抖音无水印视频下载与文案提取

[![PyPI version](https://badge.fury.io/py/douyin-mcp-server.svg)](https://badge.fury.io/py/douyin-mcp-server)
[![Python version](https://img.shields.io/pypi/pyversions/douyin-mcp-server.svg)](https://pypi.org/project/douyin-mcp-server/)
[![License](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](https://opensource.org/licenses/Apache-2.0)

从抖音分享链接下载无水印视频，提取音频并转换为文本。

<a href="https://glama.ai/mcp/servers/@yzfly/douyin-mcp-server">
  <img width="380" height="200" src="https://glama.ai/mcp/servers/@yzfly/douyin-mcp-server/badge" alt="douyin-mcp-server MCP server" />
</a>

## 📦 两种使用方式

| 方式 | 适用场景 | 说明 |
|------|----------|------|
| **MCP Server** | Claude Desktop、Cherry Studio 等 | MCP 协议集成 |
| **Claude Code Skill** | Claude Code CLI | 命令行批量提取，Markdown 输出 |

## ✨ 功能特性

- 🎵 **无水印视频获取** - 从抖音分享链接获取高质量无水印视频（无需 API 密钥）
- 🎧 **智能音频提取** - 自动从视频中提取音频内容
- 📝 **AI 文本识别** - 使用硅基流动 SenseVoice API 提取文案
- 📄 **Markdown 输出** - 文案自动保存为 Markdown 格式（Skill 模式）
- 🧹 **自动清理** - 智能清理处理过程中的临时文件

---

## 🚀 方式一：MCP Server (v1.3.0)

适用于 Claude Desktop、Cherry Studio 等支持 MCP 协议的应用。

### 步骤 1：获取 API 密钥

前往 [硅基流动](https://cloud.siliconflow.cn/i/TxUlXG3u) 注册并获取 API Key（有免费额度）。

### 步骤 2：配置 MCP Server

```json
{
  "mcpServers": {
    "douyin-mcp": {
      "command": "uvx",
      "args": ["douyin-mcp-server"],
      "env": {
        "API_KEY": "sk-xxxx"
      }
    }
  }
}
```

### 工具说明

| 工具 | 功能 | 需要 API |
|------|------|----------|
| `get_douyin_download_link` | 获取无水印下载链接 | ❌ |
| `extract_douyin_text` | 提取视频文案 | ✅ |
| `parse_douyin_video_info` | 解析视频信息 | ❌ |

---

## 🛠️ 方式二：Claude Code Skill

适用于 Claude Code CLI，支持批量提取文案并保存为 Markdown 文件。

### 安装 Skill

下载 `douyin-video.skill` 文件，解压到 skills 目录：

```bash
# 全局安装
unzip douyin-video.skill -d ~/.claude/skills/

# 或项目级安装
unzip douyin-video.skill -d .claude/skills/
```

### 配置环境变量

```bash
export API_KEY="your-siliconflow-api-key"
```

### 使用方法

```bash
# 获取视频信息（无需 API）
python douyin_downloader.py -l "抖音链接" -a info

# 下载视频
python douyin_downloader.py -l "抖音链接" -a download -o ./videos

# 提取文案
python douyin_downloader.py -l "抖音链接" -a extract -o ./output

# 提取文案并保存视频
python douyin_downloader.py -l "抖音链接" -a extract -o ./output --save-video
```

### 输出格式

```
output/
├── 7600361826030865707/
│   └── transcript.md       # Markdown 格式文案
└── 7581044356631612699/
    ├── transcript.md
    └── *.mp4               # --save-video 时保存
```

**transcript.md 示例：**

```markdown
# 视频标题

| 属性 | 值 |
|------|-----|
| 视频ID | `7600361826030865707` |
| 提取时间 | 2026-01-30 14:19:00 |
| 下载链接 | [点击下载](url) |

---

## 文案内容

(语音识别文字)
```

---

## 📋 系统要求

- **Python**: 3.8+
- **FFmpeg**: 必须安装
  - macOS: `brew install ffmpeg`
  - Ubuntu: `apt install ffmpeg`

---

## 🔧 本地开发

```bash
git clone https://github.com/yzfly/douyin-mcp-server.git
cd douyin-mcp-server
pip install -e .
python -m douyin_mcp_server.server
```

---

## ⚠️ 免责声明

- 本项目仅供学习和研究使用
- 使用者需遵守相关法律法规
- 禁止用于侵犯知识产权的行为
- 作者不对使用本项目产生的损失承担责任

---

## 📝 更新日志

### v1.3.0 (最新)

- ✨ **Claude Code Skill**：新增命令行工具，支持批量提取文案
- 📄 **Markdown 输出**：文案自动保存为 Markdown 格式
- 🎯 **双 API 支持**：同时支持硅基流动和阿里云百炼 API

### v1.2.0

- 🔄 **API 切换**：升级为阿里云百炼 API（环境变量 `DASHSCOPE_API_KEY`）

### v1.1.0

- 🐛 **问题修复**：修复文件名过长导致的错误

### v1.0.0

- 🎉 首次发布

---

## 📄 许可证

Apache License 2.0

## 👨‍💻 作者

**yzfly** - [GitHub](https://github.com/yzfly) | [Email](mailto:yz.liu.me@gmail.com)
