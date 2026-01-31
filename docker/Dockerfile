FROM dev_rs2:rs2_v2 AS builder

SHELL ["/bin/bash", "-c"]
ENV DEBIAN_FRONTEND=noninteractive
ENV PYTHON_EXECUTABLE=/usr/bin/python3
ENV PATH=/usr/bin:$PATH


RUN apt-get update && apt-get install -y \
    python3-catkin-pkg \
    python3-colcon-common-extensions \
 && rm -rf /var/lib/apt/lists/*

WORKDIR /docker_rs2_ws

COPY rs2_ws/src/ /docker_rs2_ws/src/

RUN which colcon && \
    which python3 && \
    python3 -c "import sys; print(sys.executable)"



# 构建
RUN source /opt/ros/jazzy/setup.bash && \
#    colcon build --merge-install
     colcon build --mixin --parallel-workers 8 --executor sequential
#     colcon build --merge-install
FROM dev_rs2:rs2_v2
SHELL ["/bin/bash", "-c"]

WORKDIR /docker_rs2_ws
# 只拷贝 install（精髓）
COPY --from=builder /docker_rs2_ws/install /docker_rs2_ws/install

# 自动 source
RUN echo "source /docker_rs2_ws/install/setup.bash" >> /root/.bashrc


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

CMD ["bash"]


RUN rm -rf /docker_rs2_ws/build  /docker_rs2_ws/log
