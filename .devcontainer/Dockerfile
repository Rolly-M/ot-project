FROM mcr.microsoft.com/devcontainers/rust:0-1-bullseye
RUN sed -i 's/bullseye/testing/g' /etc/apt/sources.list \
    && dpkg --add-architecture arm64 \
    && apt-get -qq update && export DEBIAN_FRONTEND=noninteractive \
    && apt-get -qq -y install libaom-dev libdav1d-dev libclang-dev \
        libaom-dev:arm64 libdav1d-dev:arm64 gcc-aarch64-linux-gnu \
        nasm qemu-user cargo-c dav1d libgit2-dev libfuzzer-14-dev \
        < /dev/null > /dev/null && apt-get -qq clean \
    && rm -rf /var/lib/apt/lists/* /var/log/apt/* /var/log/dpkg.log
RUN rustup target add aarch64-unknown-linux-gnu
RUN cargo install -q cargo-criterion \
    && cargo install -q cargo-fuzz \
    && rm -rf /usr/local/cargo/registry
RUN curl -sSf https://downloadmirror.intel.com/751535/sde-external-9.14.0-2022-10-25-lin.tar.xz | tar Jx -C /opt \
    && ln -sv /opt/sde-external-9.14.0-2022-10-25-lin/sde64 /usr/local/bin/sde64
ENV CARGO_TARGET_AARCH64_UNKNOWN_LINUX_GNU_RUNNER=qemu-aarch64
ENV CARGO_TARGET_AARCH64_UNKNOWN_LINUX_GNU_RUSTFLAGS=-Clinker=aarch64-linux-gnu-gcc
ENV PKG_CONFIG_ALLOW_CROSS_aarch64_unknown_linux_gnu=1
ENV PKG_CONFIG_PATH_aarch64_unknown_linux_gnu=/usr/lib/aarch64-linux-gnu/pkgconfig
ENV PKG_CONFIG_SYSROOT_DIR_aarch64_unknown_linux_gnu=/
ENV CUSTOM_LIBFUZZER_PATH=/usr/lib/llvm-14/lib/libFuzzer.a
ENV CARGO_TARGET_X86_64_UNKNOWN_LINUX_GNU_RUNNER="sde64 -icl --"
LABEL dev.containers.source=https://github.com/barrbrain/rav1e
LABEL dev.containers.id=rav1e
LABEL org.opencontainers.image.source=https://github.com/barrbrain/rav1e
LABEL org.opencontainers.image.description="rav1e devcontainer"
LABEL org.opencontainers.image.licenses=MIT
