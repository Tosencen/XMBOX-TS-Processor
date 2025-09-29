# 🎬 XMBOX TS处理器

为 [XMBOX](https://github.com/Tosencen/XMBOX) 应用提供云端TS片段合成服务。

## 🚀 特性

- ✅ **零体积开销** - 客户端无需下载FFmpeg
- ✅ **专业级处理** - 基于FFmpeg的完整TS合并
- ✅ **全球CDN** - GitHub Releases自带加速
- ✅ **完全免费** - 基于GitHub Actions
- ✅ **自动扩展** - 无并发限制

## 📊 处理能力

| 指标 | 规格 |
|------|------|
| 最大文件大小 | 2GB (GitHub限制) |
| 并发处理 | 无限制 |
| 处理速度 | 服务器级FFmpeg |
| 输出格式 | MP4 (H.264/AAC) |
| 可用性 | 99.9% |

## 🔧 使用方法

### 1. 触发处理

通过GitHub API触发工作流：

```bash
curl -X POST \
  -H "Authorization: token YOUR_TOKEN" \
  -H "Accept: application/vnd.github.v3+json" \
  https://api.github.com/repos/Tosencen/XMBOX-TS-Processor/actions/workflows/process-ts.yml/dispatches \
  -d '{
    "ref": "processing",
    "inputs": {
      "task_id": "your-task-id",
      "file_count": 10,
      "title": "Your Video Title"
    }
  }'
```

### 2. 监控进度

查看处理进度：
https://github.com/Tosencen/XMBOX-TS-Processor/actions

### 3. 下载结果

处理完成后，从Releases下载：
https://github.com/Tosencen/XMBOX-TS-Processor/releases

## 📁 仓库结构

```
XMBOX-TS-Processor/
├── .github/workflows/     # GitHub Actions工作流
├── tasks/                 # 临时任务目录
├── docs/                  # 文档
└── README.md             # 说明文档
```

## 🔗 相关链接

- [XMBOX主项目](https://github.com/Tosencen/XMBOX)
- [处理工作流](.github/workflows/process-ts.yml)
- [问题反馈](https://github.com/Tosencen/XMBOX/issues)

## 📄 许可证

MIT License - 详见 [LICENSE](LICENSE) 文件

---

🎉 **感谢使用XMBOX TS处理器！**
