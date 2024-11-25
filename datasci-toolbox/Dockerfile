# Use an official NVIDIA CUDA base image with Python
FROM nvidia/cuda:11.8.0-cudnn8-runtime-ubuntu20.04

# Install necessary packages
RUN apt-get update && \
    apt-get install -y python3-pip git curl wget vim && \
    rm -rf /var/lib/apt/lists/*

# Set up Python environment
RUN pip3 install --upgrade pip
RUN pip3 install tensorflow==2.10.0 jupyterlab

# Install VSCode Server
RUN mkdir -p /home/vscode/.vscode-server/bin && \
    wget https://github.com/coder/code-server/releases/download/v4.95.3/code-server-4.95.3-linux-amd64.tar.gz -O vscode-server-linux-x64.tar.gz && \
    tar -xvzf vscode-server-linux-x64.tar.gz -C /home/vscode/.vscode-server/ --strip-components=1 && \
    rm vscode-server-linux-x64.tar.gz

# Set up VSCode Server environment
ENV VSCODE_SERVER=/home/vscode/.vscode-server/bin/*
ENV PATH=$VSCODE_SERVER:$PATH

# Create a user for VSCode Server
RUN useradd -m vscode && \
    chown -R vscode:vscode /home/vscode

USER vscode
WORKDIR /home/vscode

# Expose the port for VSCode Server
EXPOSE 8080

# Start VSCode Server
CMD ["/home/vscode/.vscode-server/bin/code-server", "--bind-addr=0.0.0.0:8080", "--auth=none"]
