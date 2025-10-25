# 使用Python 3.12官方镜像作为基础镜像
FROM docker.1ms.run/python:3.12.1

# 设置工作目录
WORKDIR /app

# 设置环境变量
ENV PYTHONDONTWRITEBYTECODE=1
ENV PYTHONUNBUFFERED=1

# 配置pip中国镜像源
RUN pip config set global.index-url https://pypi.tuna.tsinghua.edu.cn/simple/ \
    && pip config set global.trusted-host pypi.tuna.tsinghua.edu.cn

# 配置apt中国镜像源（完全替换方法）
RUN rm -rf /etc/apt/sources.list.d/* && \
    echo "deb https://mirrors.tuna.tsinghua.edu.cn/debian/ bookworm main contrib non-free non-free-firmware" > /etc/apt/sources.list && \
    echo "deb https://mirrors.tuna.tsinghua.edu.cn/debian/ bookworm-updates main contrib non-free non-free-firmware" >> /etc/apt/sources.list && \
    echo "deb https://mirrors.tuna.tsinghua.edu.cn/debian-security bookworm-security main contrib non-free non-free-firmware" >> /etc/apt/sources.list && \
    find /etc/apt -name "*.list" -exec sed -i 's|http://deb.debian.org|https://mirrors.tuna.tsinghua.edu.cn|g' {} \; && \
    find /etc/apt -name "*.list" -exec sed -i 's|http://security.debian.org|https://mirrors.tuna.tsinghua.edu.cn|g' {} \;

# 安装系统依赖
RUN apt-get update \
    && apt-get install -y --no-install-recommends \
        gcc \
        build-essential \
        python3-dev \
        libffi-dev \
        libssl-dev \
    && rm -rf /var/lib/apt/lists/*

# 复制依赖文件并安装Python依赖
COPY ../requirements.txt .
RUN pip install --no-cache-dir --upgrade pip \
    && pip install --no-cache-dir -r requirements.txt

# 复制应用代码
COPY . .

# 创建必要的目录
RUN mkdir -p logs/api_log logs/exception_log

# 暴露端口
EXPOSE 8088

# 启动命令
CMD ["python", "app.py"]