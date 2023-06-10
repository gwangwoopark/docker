FROM ubuntu:22.04

ARG USER_NAME="ubuntu"
ARG USER_PASSWORD=${USER_NAME}
ARG HOME="/home/${USER_NAME}"

ARG GIT_USER="Gwangwoo Park"
ARG GIT_EMAIL="gwangwoo.park@gmail.com"

ENV DEBIAN_FRONTEND="noninteractive"

RUN apt update && apt install -y zsh 

RUN apt install -y curl git wget bat 
RUN curl -sLf https://spacevim.org/install.sh | bash

# add a user (--disabled-password: the user won't be able to use the account until the password is set)
RUN adduser ${USER_NAME} --quiet --disabled-password --shell /bin/zsh --gecos "" && chown -R ${USER_NAME}:${USER_NAME} ${HOME}
# update the password
RUN echo "${USER_NAME}:${USER_PASSWORD}" | chpasswd && usermod -g ${USER_NAME} -aG sudo ${USER_NAME}

USER ${USER_NAME}
RUN sh -c "$(curl -fsSL https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
RUN git clone --depth=1 https://github.com/romkatv/powerlevel10k.git ${ZSH_CUSTOM:-$HOME/.oh-my-zsh/custom}/themes/powerlevel10k
RUN git clone https://github.com/zsh-users/zsh-completions ${ZSH_CUSTOM:-${ZSH:-~/.oh-my-zsh}/custom}/plugins/zsh-completions
RUN git clone https://github.com/zsh-users/zsh-syntax-highlighting ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting
RUN git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions
RUN git clone https://github.com/zsh-users/zsh-history-substring-search ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-history-substring-search

USER root
COPY .docker.zshrc ${HOME}/.zshrc
COPY .docker.p10k.zsh ${HOME}/.p10k.zsh
RUN chown ${GROUP_NAME}:${USER_NAME} ${HOME}/.zshrc ${HOME}/.p10k.zsh

USER ${USER_NAME}

RUN git config --global user.name ${GIT_USER} \
  && git config --global user.email ${GIT_EMAIL} \
  && git config --global core.editor "vim"

WORKDIR ${HOME}
ENTRYPOINT [ "zsh" ]
