FROM mcr.microsoft.com/devcontainers/base:bookworm

RUN export DEBIAN_FRONTEND=noninteractive 
RUN echo "deb [arch=amd64] http://download.proxmox.com/debian/pve bookworm pve-no-subscription" > /etc/apt/sources.list.d/pve-install-repo.list 
RUN echo "deb http://security.debian.org/debian-security bookworm-security main contrib" >> /etc/apt/sources.list.d/pve-install-repo.list 
RUN wget https://enterprise.proxmox.com/debian/proxmox-release-bookworm.gpg -O /etc/apt/trusted.gpg.d/proxmox-release-bookworm.gpg 
RUN apt-get update 
RUN apt-get install -y wget git lsb-release wget software-properties-common gnupg 
RUN apt-get install -y build-essential devscripts debhelper dh-python asciidoc-base bc bison cpio dwarves flex kmod libdw-dev libiberty-dev libnuma-dev libslang2-dev lz4 rsync xmlto 
RUN apt-get install -y libunwind-dev libpfm4-dev coccinelle openjdk-17-jdk libcapstone-dev libbabeltrace-dev systemtap-sdt-dev libzstd-dev dwarves zstd libbfd-dev libperl-dev
RUN wget https://apt.llvm.org/llvm.sh 
RUN chmod +x llvm.sh 
RUN ./llvm.sh 
RUN apt-get install -y clang-18 lldb-18 lld-18 clangd-18 
RUN ln -s /usr/bin/clang++-18 /usr/bin/clang++ 
RUN ln -s /usr/bin/clang-18 /usr/bin/clang 
RUN ln -s /usr/bin/llvm-18 /usr/bin/llvm 
RUN ln -s /usr/bin/lld-18 /usr/bin/lld 
RUN ln -s /usr/bin/llvm-nm-18 /usr/bin/llvm-nm 
RUN ln -s /usr/bin/llvm-objcopy-18 /usr/bin/llvm-objcopy 
RUN ln -s /usr/bin/llvm-objdump-18 /usr/bin/llvm-objdump 
RUN ln -s /usr/bin/llvm-readelf-18 /usr/bin/llvm-readelf 
RUN ln -s /usr/bin/llvm-ar-18 /usr/bin/llvm-ar 
RUN ln -s /usr/bin/llvm-strip-18 /usr/bin/llvm-strip 
RUN ln -s /usr/bin/ld.lld-18 /usr/bin/ld.lld
RUN ln -s /usr/lib/llvm-18 /usr/lib/llvm
RUN git config --global safe.directory '*' 
RUN apt-get install -y cmake python3 python3-matplotlib zlib1g-dev libelf-dev 
RUN apt-get install -y libpve-common-perl
RUN git clone https://github.com/acmel/dwarves.git --recursive --branch v1.29 --single-branch
RUN apt-get install -y pahole
RUN cmake -S dwarves -B dwarves_build -G "Unix Makefiles" -D __LIB=lib -D CMAKE_BUILD_TYPE=Release -D CMAKE_INSTALL_PREFIX=/usr -DBUILD_SHARED_LIBS=OFF
RUN cmake --build dwarves_build
RUN cmake --install dwarves_build
RUN mv /usr/bin/nproc /usr/bin/nproc.bak
RUN echo '#include <stdio.h>' > fake_nproc.c
RUN echo 'int main() {printf("4\\n");return 0;}' >> fake_nproc.c
RUN clang fake_nproc.c -o fake_nproc
RUN mv fake_nproc /usr/bin/nproc
RUN apt-get install -y libperl-dev