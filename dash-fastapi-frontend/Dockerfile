# 使用Python 3.12官方镜像作为基础镜像
FROM docker.1ms.run/python:3.12.1

# 设置工作目录
WORKDIR /app

# 设置环境变量
ENV PYTHONDONTWRITEBYTECODE=1
ENV PYTHONUNBUFFERED=1

# 安装系统依赖
RUN apt-get update \
    && apt-get install -y --no-install-recommends \
        gcc \
        g++ \
        curl \
        build-essential \
        python3-dev \
        libffi-dev \
        libssl-dev \
    && rm -rf /var/lib/apt/lists/*

# 复制依赖文件并安装Python依赖
COPY ../requirements.txt .
RUN pip install --no-cache-dir --upgrade pip \
    && pip install --no-cache-dir cython \
    && pip install --no-cache-dir -r requirements.txt

# 复制应用代码
COPY . .

# 创建必要的目录
RUN mkdir -p logs/api_log logs/exception_log

# 暴露端口
EXPOSE 8088

# 启动命令
CMD ["python", "app.py"]