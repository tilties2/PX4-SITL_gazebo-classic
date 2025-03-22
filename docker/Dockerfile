FROM gazebo:gzserver11-focal

# Switch to root (if not already)
USER root

# Install additional packages:
# - Python and common utilities
# - Mesa libraries and DRI packages for GLX
# - Xvfb to provide a virtual display if needed
# - NVIDIA packages might be required if not provided by the runtime (adjust as needed)
RUN apt-get update && apt-get install -y \
    python3 \
    python3-pip \
    git \
    wget \
    nano \
    cmake \
    libgl1-mesa-glx \
    libgl1-mesa-dri \
    libglu1-mesa \
    mesa-utils \
    xvfb \
    libboost-all-dev \
    libgazebo11-dev \
    libopencv-dev \
    libgstreamer1.0-dev libgstreamer-plugins-base1.0-dev \
    && rm -rf /var/lib/apt/lists/*

# mavlink
WORKDIR /root/third_parties
RUN git clone https://github.com/mavlink/mavlink.git
WORKDIR /root/third_parties/mavlink
RUN git submodule update --init --recursive
WORKDIR /root/third_parties/mavlink/build
RUN cmake ..
RUN make -j4
RUN make install

# Set NVIDIA-related environment variables for GPU support
ENV NVIDIA_VISIBLE_DEVICES=all
ENV NVIDIA_DRIVER_CAPABILITIES=graphics,compute,utility

# Copy entrypoint script
WORKDIR /
COPY entrypoint.sh /entrypoint.sh
RUN chmod +x /entrypoint.sh

# Start with the entrypoint script, then run gazebo
ENTRYPOINT ["/entrypoint.sh"]
SHELL ["/bin/bash", "-c"]
CMD ["bash"]
# CMD ["gazebo", "--verbose"]
