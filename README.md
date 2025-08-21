# LMDeploy-Jetson
LMDeploy jetson docker镜像构建，基于dustynv大神的vllm镜像构建。
### Dockerfile:
```Dockerfile
# 基础镜像
FROM dustynv/vllm:0.9.2-r36.4-cu128-24.04

# 设置构建时可选代理参数
ARG HTTP_PROXY
ARG HTTPS_PROXY

# 设置代理环境变量（国内网络必须使用代理）
ENV http_proxy=${HTTP_PROXY}
ENV https_proxy=${HTTPS_PROXY}
ENV HTTP_PROXY=${HTTP_PROXY}
ENV HTTPS_PROXY=${HTTPS_PROXY}

# 切换到 bash
SHELL ["/bin/bash", "-c"]

# 安装 LMDeploy (源码安装)
RUN pip install git+https://github.com/InternLM/lmdeploy.git \
    --index-url https://pypi.jetson-ai-lab.io/jp6/cu128 \
    --extra-index-url https://mirrors.ustc.edu.cn/pypi/simple/

CMD ["tail", "-f", "/dev/null"]

```
### 构建示例：
```bash
docker build -t lmdeploy-jetson-test:v1.0 \
  --build-arg HTTP_PROXY=http://172.16.16.47:7890 \
  --build-arg HTTPS_PROXY=http://172.16.16.47:7890 \
  .
```

| 如果基础镜像版本落后，可访问：https://hub.docker.com/r/dustynv/vllm/tags 获取dustynv大神的最新基础镜像。
