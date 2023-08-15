FROM pytorch/pytorch:2.0.1-cuda11.7-cudnn8-devel
# ARG HUGGING_FACE_HUB_TOKEN
ENV DEBIAN_FRONTEND noninteractive
# gradio and streamlit default ports
# EXPOSE 7860 8501
# EXPOSE 7500-8999

### Set default shell to /bin/bash ###
SHELL ["/bin/bash", "-euo", "pipefail", "-c"]

# Install some basic utilities
RUN apt-get update && apt-get install python3 python3-pip -y

# Install some basic utilities
RUN apt-get update && apt-get install -y \
    curl \
    ca-certificates \
    sudo \
    git \
    bzip2 \
    libx11-6 \
    build-essential \
    wget \
    unzip \
    nvidia-cuda-toolkit \
 && rm -rf /var/lib/apt/lists/*

### Set default NCCL parameters ###
RUN echo NCCL_DEBUG=INFO >> /etc/nccl.conf

ENV LD_LIBRARY_PATH=/usr/local/cuda/lib64:$LD_LIBRARY_PATH
# WORKDIR /home/user
# WORKDIR /home/user/app
### Mount Point ###
# When launching the container, mount the code directory to /app
ARG APP_MOUNT=/app
ARG MODEL_WEIGHT=/app/weight
VOLUME [ ${APP_MOUNT}, ${MODEL_WEIGHT} ]
WORKDIR ${APP_MOUNT}
COPY ./requirements.docker.txt /requirements.docker.txt
RUN python3 -m pip install --no-cache-dir -r /requirements.docker.txt
RUN python3 -m pip install flash-attn --no-build-isolation
RUN python3 -m pip install deepspeed==0.9.2 accelerate==0.21.0 bitsandbytes==0.41.1 scipy
# ENTRYPOINT ["bash", "launch.sh"]