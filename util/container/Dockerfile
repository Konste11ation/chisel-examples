# Dockerfile for the chisel
FROM ubuntu:24.04 AS build-bender

RUN apt-get update && apt-get install -y lsb-release wget software-properties-common gnupg curl && \
    echo "deb https://repo.scala-sbt.org/scalasbt/debian all main" | tee /etc/apt/sources.list.d/sbt.list && \
    echo "deb https://repo.scala-sbt.org/scalasbt/debian /" | tee /etc/apt/sources.list.d/sbt_old.list && \
    curl -sL "https://keyserver.ubuntu.com/pks/lookup?op=get&search=0x2EE0EA64E40A89B84B2DF73499E82A75642AC823" | apt-key add && \
    apt-get update && \
    apt-get upgrade -y && \
    apt-get install -y \
    wget \
    curl \
    build-essential \
    git \
    zsh \
    nano \
    openjdk-11-jre-headless openjdk-11-jdk-headless \
    help2man perl python3 make autoconf g++ flex bison ccache\
    libgoogle-perftools-dev numactl perl-doc \
    libfl2 \
    libfl-dev \
    zlib1g zlib1g-dev \
    sbt \
    python3-pip && \
    python3 -m pip config set global.break-system-packages true

# Install scala-cli
RUN curl -sSLf https://scala-cli.virtuslab.org/get | sh && \
# Install mill build tool
    curl -L https://raw.githubusercontent.com/lefou/millw/0.4.11/millw > mill && chmod +x mill && mv mill /usr/local/bin && \
# Remove cache
    rm -rf /root/.cache

# Install Verilator
RUN git clone https://github.com/verilator/verilator && \
  cd verilator && \
  git checkout v5.024 && \
  unset VERILATOR_ROOT && \
  autoconf && \
  ./configure && \
  make -j$(nproc) && \
  make install && \
  cd .. && \
  rm -rf verilator && \
  rm -rf /root/.cache

# vcd-to-csv
RUN wget https://github.com/KULeuven-MICAS/vcd-to-csv/releases/latest/download/vcd-to-csv -P /usr/bin && chmod +x /usr/bin/vcd-to-csv


# Install Oh-My-Zsh and Autocomplete Plugin
RUN sh -c "$(curl -fsSL https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/install.sh)" "" --unattended && \
    git clone https://github.com/zsh-users/zsh-autosuggestions.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions && \
    echo "source ~/.oh-my-zsh/custom/plugins/zsh-autosuggestions/zsh-autosuggestions.zsh" >> ~/.zshrc && \
    chsh -s $(which zsh)

# Zsh as default shell
CMD ["zsh"]

WORKDIR /