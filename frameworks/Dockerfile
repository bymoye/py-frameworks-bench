# 使用基础 Python 镜像
FROM python:3.13

# 安装必要的依赖
RUN apt-get update && \
    apt-get -y install --no-install-recommends build-essential && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

# 禁用 pip 版本检查以加快安装速度
ENV PIP_DISABLE_PIP_VERSION_CHECK=1

# 安装常用的 Python 包
RUN /usr/local/bin/pip install --no-cache-dir \
    wheel \
    gunicorn \
    orjson \
    ujson \
    uvicorn[standard]

# 动态设置项目路径
ARG PROJECT
WORKDIR /app

# 将项目文件复制到容器中
COPY . /app/

# 确保 start.sh 脚本可执行
RUN chmod +x /app/start.sh

# 暴露服务端口
EXPOSE 8080

# 定义容器入口点
ENTRYPOINT ["/app/start.sh"]