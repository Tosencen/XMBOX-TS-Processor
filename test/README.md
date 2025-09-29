# 测试说明

## 本地测试

1. 准备测试文件：
```bash
# 创建测试目录
mkdir -p test/sample

# 生成测试TS文件
for i in {1..5}; do
  ffmpeg -f lavfi -i "testsrc=duration=10:size=1280x720:rate=30" \
         -c:v libx264 -preset ultrafast \
         "test/sample/test_$(printf %04d $i).ts"
done
```

2. 测试处理流程：
```bash
# 设置任务ID
TASK_ID=$(uuidgen)

# 创建任务目录
mkdir -p "tasks/$TASK_ID"

# 复制测试文件
cp test/sample/*.ts "tasks/$TASK_ID/"

# 生成文件列表
cd "tasks/$TASK_ID"
for f in *.ts; do
  echo "file '$f'" >> files.txt
done

# 测试合并
OUTPUT_FILE="../../output/${TASK_ID}.mp4"
ffmpeg -f concat -safe 0 -i files.txt -c copy -movflags +faststart "$OUTPUT_FILE"
```

## API测试

1. 设置环境变量：
```bash
export GITHUB_TOKEN="your_token_here"
export REPO_OWNER="Tosencen"
export REPO_NAME="XMBOX-TS-Processor"
```

2. 触发处理：
```bash
curl -X POST \
  -H "Accept: application/vnd.github.v3+json" \
  -H "Authorization: Bearer $GITHUB_TOKEN" \
  -d '{
    "event_type": "process_ts",
    "client_payload": {
      "task_id": "'$(uuidgen)'",
      "file_count": 5,
      "title": "测试视频"
    }
  }' \
  "https://api.github.com/repos/$REPO_OWNER/$REPO_NAME/dispatches"
```

3. 检查结果：
```bash
curl -s \
  -H "Accept: application/vnd.github.v3+json" \
  -H "Authorization: Bearer $GITHUB_TOKEN" \
  "https://api.github.com/repos/$REPO_OWNER/$REPO_NAME/releases/latest"
```

## 注意事项

1. 测试文件应该：
   - 大小合适（建议每个文件1-10MB）
   - 编码正确（h264/aac）
   - 分段完整

2. 测试过程应该验证：
   - 文件上传是否成功
   - 处理是否正常完成
   - 输出文件是否可以播放
   - 进度回调是否正常

3. 异常测试应该包括：
   - 无效的TS文件
   - 网络超时
   - 权限错误
   - 空文件处理