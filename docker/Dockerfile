FROM ubuntu:24.04

SHELL ["/bin/bash", "-c"]
ENV DEBIAN_FRONTEND=noninteractive

# 1️⃣ 系统依赖（容器内部）
RUN apt update && apt install -y \
    python3 \
    python3-venv \
    python3-pip \
    libgl1 \
    libglib2.0-0 \
    ca-certificates \
    && rm -rf /var/lib/apt/lists/*

# 2️⃣ 创建 venv（容器内部）
RUN python3 -m venv /opt/venv

# 3️⃣ 安装 YOLOv11（容器内部）
RUN /opt/venv/bin/pip install --upgrade pip
RUN /opt/venv/bin/pip install ultralytics opencv-python

# 4️⃣ ⭐ 关键：让 venv 成为默认 Python
ENV PATH="/opt/venv/bin:$PATH"

# 5️⃣ 仅为交互体验（非必须）
RUN echo "source /opt/venv/bin/activate" >> /root/.bashrc

WORKDIR /workspace
CMD ["bash"]
