# 📡 XMBOX TS处理器 API文档

## 🔗 API端点

### 基础URL
```
https://api.github.com/repos/Tosencen/XMBOX-TS-Processor
```

## 🚀 工作流触发

### 触发TS处理
```http
POST /actions/workflows/process-ts.yml/dispatches
```

**请求头:**
```
Authorization: token YOUR_GITHUB_TOKEN
Accept: application/vnd.github.v3+json
Content-Type: application/json
```

**请求体:**
```json
{
  "ref": "processing",
  "inputs": {
    "task_id": "unique-task-id",
    "file_count": 10,
    "title": "视频标题"
  }
}
```

**参数说明:**
- `task_id`: 唯一任务标识符
- `file_count`: TS文件数量
- `title`: 视频标题

## 📊 状态查询

### 获取工作流运行状态
```http
GET /actions/runs
```

**查询参数:**
- `per_page`: 每页数量 (默认30)
- `page`: 页码 (默认1)

### 获取特定运行状态
```http
GET /actions/runs/{run_id}
```

## 📥 结果下载

### 获取Releases列表
```http
GET /releases
```

### 下载处理结果
处理完成后，结果文件会发布到GitHub Releases，下载链接格式：
```
https://github.com/Tosencen/XMBOX-TS-Processor/releases/download/{task_id}/{task_id}.mp4
```

## 🔄 处理流程

1. **上传TS文件** → GitHub Contents API
2. **触发工作流** → GitHub Actions API
3. **监控状态** → GitHub Actions API
4. **下载结果** → GitHub Releases API

## 📝 示例代码

### JavaScript/Node.js
```javascript
const axios = require('axios');

async function processTS(taskId, fileCount, title) {
  const response = await axios.post(
    'https://api.github.com/repos/Tosencen/XMBOX-TS-Processor/actions/workflows/process-ts.yml/dispatches',
    {
      ref: 'processing',
      inputs: {
        task_id: taskId,
        file_count: fileCount,
        title: title
      }
    },
    {
      headers: {
        'Authorization': 'token YOUR_GITHUB_TOKEN',
        'Accept': 'application/vnd.github.v3+json'
      }
    }
  );
  
  return response.data;
}
```

### Python
```python
import requests

def process_ts(task_id, file_count, title):
    url = "https://api.github.com/repos/Tosencen/XMBOX-TS-Processor/actions/workflows/process-ts.yml/dispatches"
    
    headers = {
        "Authorization": "token YOUR_GITHUB_TOKEN",
        "Accept": "application/vnd.github.v3+json"
    }
    
    data = {
        "ref": "processing",
        "inputs": {
            "task_id": task_id,
            "file_count": file_count,
            "title": title
        }
    }
    
    response = requests.post(url, json=data, headers=headers)
    return response.json()
```

## ⚠️ 注意事项

1. **认证**: 需要GitHub Personal Access Token
2. **限制**: 遵循GitHub API速率限制
3. **文件大小**: 单个文件最大2GB
4. **存储**: 处理结果24小时后自动清理

## 🔗 相关链接

- [GitHub Actions API](https://docs.github.com/en/rest/actions)
- [GitHub Contents API](https://docs.github.com/en/rest/repos/contents)
- [GitHub Releases API](https://docs.github.com/en/rest/releases)
