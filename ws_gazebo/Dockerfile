FROM osrf/ros:jazzy-desktop-full

ARG USERNAME=ros
ARG USER_UID=1010
ARG USER_GID=$USER_UID

RUN apt-get update && apt-get install -y sudo 
RUN sudo groupadd --gid $USER_GID $USERNAME 
RUN sudo useradd -s /bin/bash --uid $USER_UID --gid $USER_GID -m $USERNAME 
  
# Set permissions on the home directory explicitly
# RUN mkdir -p /home/$USERNAME && chown -R $USER_UID:$USER_GID /home/$USERNAME

RUN echo $USERNAME ALL=\(root\) NOPASSWD:ALL > /etc/sudoers.d/$USERNAME \
    && chmod 0440 /etc/sudoers.d/$USERNAME

RUN echo "source /opt/ros/jazzy/setup.bash" >> /home/$USERNAME/.bashrc