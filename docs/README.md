# Veloera Docs

这**不是**完整文档！部分自动化文档脚本会将成果输出到此目录。

查看 `Veloera/docs` 仓库以查看文档源码，可在 https://docs.veloera.org/ 查看完整文档！

## jenkins 构建脚本,image push registry.cn-shanghai.aliyuncs.com/jsonsong-pub/my-api:{VERSION}

```bash
#!/bin/bash -l

# 读取版本号
VERSION=$(cat VERSION)
IMAGE_NAME="registry.cn-shanghai.aliyuncs.com/jsonsong-pub/my-api:${VERSION}"

echo "构建版本: ${VERSION}"
echo "镜像名称: ${IMAGE_NAME}"

# 使用 buildx 构建并推送镜像
docker buildx build \
    --platform linux/amd64 \
    --cache-from=type=local,src=/tmp/.buildx-cache \
    --cache-to=type=local,dest=/tmp/.buildx-cache,mode=max \
    -t ${IMAGE_NAME} \
    -t registry.cn-shanghai.aliyuncs.com/jsonsong-pub/my-api:latest \
    --push \
    .

if [ $? -eq 0 ]; then
    echo "构建成功！镜像已推送: ${IMAGE_NAME}"
else
    echo "构建失败！"
    exit 1
fi
```
