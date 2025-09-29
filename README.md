# XMBOX TS处理器

这是一个用于XMBOX应用的TS视频处理服务。它使用GitHub Actions来处理TS视频文件的合并和转换工作。

## 功能特点

- 支持多个TS片段合并
- 使用FFmpeg进行专业视频处理
- 自动发布处理结果
- 支持异步处理
- 安全的文件传输

## 工作流程

1. 客户端上传TS文件到指定位置
2. 触发GitHub Actions工作流
3. 使用FFmpeg进行专业处理
4. 自动发布处理结果
5. 客户端下载处理完成的文件

## 配置说明

### 1. 仓库设置

在仓库的Settings -> Actions -> General中：

1. 在"Workflow permissions"部分：
   - 选择 "Read and write permissions"
   - 勾选 "Allow GitHub Actions to create and approve pull requests"

2. 在"Fork pull request workflows"部分：
   - 根据需要配置工作流访问权限

### 2. Secrets配置

在仓库的Settings -> Secrets and variables -> Actions中添加以下secrets：

1. `GITHUB_TOKEN`：
   - 自动提供的token，用于工作流访问
   - 无需手动配置

### 3. 安全配置

为了确保安全，建议：

1. 限制仓库访问权限
2. 定期轮换访问令牌
3. 监控工作流使用情况
4. 设置资源使用限制

## API使用

### 触发处理

发送POST请求到：
```
https://api.github.com/repos/Tosencen/XMBOX-TS-Processor/dispatches
```

请求头：
```
Accept: application/vnd.github.v3+json
Authorization: Bearer YOUR_GITHUB_TOKEN
```

请求体：
```json
{
  "event_type": "process_ts",
  "client_payload": {
    "task_id": "unique_task_id",
    "file_count": 10,
    "title": "video_title"
  }
}
```

### 获取结果

发送GET请求到：
```
https://api.github.com/repos/Tosencen/XMBOX-TS-Processor/releases/latest
```

## 注意事项

1. 文件大小限制：
   - 单个文件不超过2GB
   - 总大小不超过10GB

2. 处理时间：
   - 通常在5-15分钟内完成
   - 大文件可能需要更长时间

3. 错误处理：
   - 失败后自动重试
   - 超过3次失败将终止任务

## 维护者

- [@Tosencen](https://github.com/Tosencen)

## 许可证

MIT License