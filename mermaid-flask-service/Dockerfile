FROM python:3.11-slim

# 设置工作目录
WORKDIR /app

# 设置 pip 使用国内镜像源（清华大学镜像站）
RUN pip config set global.index-url https://pypi.tuna.tsinghua.edu.cn/simple

# 复制 requirements.txt 并安装依赖
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

# 清理默认的 apt 源配置并强制使用阿里云源（针对 Bookworm 版本）
RUN rm -f /etc/apt/sources.list && \
    rm -rf /etc/apt/sources.list.d/* && \
    echo "deb http://mirrors.aliyun.com/debian bookworm main" > /etc/apt/sources.list && \
    echo "deb http://mirrors.aliyun.com/debian bookworm-updates main" >> /etc/apt/sources.list && \
    echo "deb http://mirrors.aliyun.com/debian-security bookworm-security main" >> /etc/apt/sources.list && \
    apt-get update && apt-get install -y \
    curl \
    libnss3 \
    software-properties-common \
    npm \
    libatk1.0-0 \
    libatk-bridge2.0-0 \
    libcups2 \
    libxcomposite1 \
    libxrandr2 \
    libxdamage1 \
    libxkbcommon0 \
    libgbm1 \
    libpango-1.0-0 \
    libcairo2 \
    libasound2 \
    libxtst6 \
    libxshmfence1 \
    && rm -rf /var/lib/apt/lists/*

#安装中文库 不然导出中文是乱码和显示不全问题
RUN apt-get update && apt-get install -y \
        chromium \
        fonts-noto-cjk \
        fonts-noto-color-emoji \
        xfonts-terminus \
        fonts-dejavu \
        fonts-freefont-ttf \
        fonts-font-awesome \
        fonts-inconsolata \
        fonts-linuxlibertine \
        && fc-cache -f \

# 设置 npm 使用淘宝镜像源并更新
RUN npm config set registry https://registry.npmmirror.com && \
    npm install -g n && \
    n 22.9.0  # 安装 Node.js 22.9.0，确保兼容 npm@latest

# 安装最新版本的 npm
RUN npm install npm@latest -g

# 安装 puppeteer 和 mermaid-cli
RUN npm install puppeteer@latest && \
    npm install -g @mermaid-js/mermaid-cli

# 复制当前目录下的所有文件到工作目录
COPY . .

# 设置容器启动时执行的命令
CMD ["python", "main.py"]