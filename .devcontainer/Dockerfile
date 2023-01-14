FROM mcr.microsoft.com/vscode/devcontainers/base:ubuntu-22.04


ARG ZIGVER="0.10.0"
ARG ZIGTARGET="x86_64"


ARG USERNAME=vscode
ENV DEBIAN_FRONTEND noninteractive
ENV DISPLAY :0
ENV LANG=en_US.UTF-8 \
    LANGUAGE=en_US.UTF-8 \
    LC_ALL=C.UTF-8 


# C++
# python
# node
# SDL2
# OpenGL
# Qt5
# GTK4
RUN apt-get update && apt-get install -y --no-install-recommends build-essential manpages-dev \
    git tar unzip zstd dosfstools curl wget vim sudo ssh tzdata ca-certificates \
    dbus dbus-x11 alsa-utils pulseaudio pulseaudio-utils mesa-utils x11-apps \
    # cmake clang clang-format clangd \
    gdb lldb nasm  \ 
    # python3 python-is-python3 python3-distutils python3-numpy python3-pip python3-tk \
    # nodejs \
    # libsdl2-dev libsdl2-image-dev libsdl2-mixer-dev libsdl2-net-dev libsdl2-ttf-dev \
    # libglu1-mesa-dev freeglut3-dev mesa-common-dev libglew-dev libglfw3-dev \
    # qtcreator qtbase5-dev qttools5-dev-tools qtbase5-examples qt5-qmake \
    # libgtk-4-bin libgtk-4-common libgtk-4-dev \
    && apt-get clean -y && rm -rf /var/lib/apt/lists


# Install Zig
RUN wget https://ziglang.org/download/${ZIGVER}/zig-linux-${ZIGTARGET}-${ZIGVER}.tar.xz
RUN tar xf zig-linux-${ZIGTARGET}-${ZIGVER}.tar.xz -C /usr/local/
RUN mv /usr/local/zig-linux-${ZIGTARGET}-${ZIGVER} /usr/local/zig
ENV PATH $PATH:/usr/local/zig

# Install Zig lsp
COPY zls.json /home/
RUN wget https://github.com/zigtools/zls/releases/download/${ZIGVER}/${ZIGTARGET}-linux.tar.zst \
&& tar -I zstd -xvf ${ZIGTARGET}-linux.tar.zst -C . --strip-components=1 \
&& chmod +x ./zls \
&& mv ./zls /usr/local/bin/ \
&& cp -f /home/zls.json /etc/zls.json \
&& rm ./${ZIGTARGET}-linux.tar.zst


USER ${USERNAME}

# Install Rust
# ENV RUST_VERSION stable
# RUN curl https://sh.rustup.rs -sSf | sh -s -- -y --default-toolchain ${RUST_VERSION}
# ENV PATH $PATH:/home/${USERNAME}/.cargo/bin
# RUN rustup install stable
# RUN rustup install nightly
# RUN rustup component add rust-analysis rust-src rustfmt clippy
# RUN cargo install cargo-watch
# RUN cargo install cargo-check

RUN mkdir -p /home/${USERNAME}/workspace
WORKDIR /home/${USERNAME}/workspace

USER root
COPY set_uid.sh /home/
ENTRYPOINT ["/home/set_uid.sh"]
COPY ./.inject_bashrc /root
RUN cat /root/.inject_bashrc >> /home/${USERNAME}/.bashrc
CMD ["/bin/sh", "-c", "echo Container started ; trap \"exit 0\" 15; while sleep 1 & wait $!; do :; done"]
