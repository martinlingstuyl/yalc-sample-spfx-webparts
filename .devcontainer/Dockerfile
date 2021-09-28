FROM node:14
ARG USERNAME=blimped-spfx
ARG USER_UID=2000
ARG USER_GID=$USER_UID

# Create the user
RUN groupadd --gid $USER_GID $USERNAME \
    && useradd --uid $USER_UID --gid $USER_GID -m $USERNAME --create-home --shell /bin/bash \
    #
    # [Optional] Add sudo support. Omit if you don't need to install software after connecting.
    && apt-get update \
    && apt-get install -y sudo \
    && apt-get install -y git \
    && echo $USERNAME ALL=\(root\) NOPASSWD:ALL > /etc/sudoers.d/$USERNAME \
    && chmod 0440 /etc/sudoers.d/$USERNAME

# ********************************************************
# * Anything else you want to do like clean up goes here *
# ********************************************************

EXPOSE 5432 4321 35729

RUN sudo npm i -g gulp@4 yo@4 @microsoft/generator-sharepoint@1.12.1 yalc --unsafe-perm 
VOLUME /workspace/blimped-spfx
WORKDIR /workspace/blimped-spfx
RUN usermod -aG sudo blimped-spfx && \
    chown -R blimped-spfx:blimped-spfx /workspace/blimped-spfx