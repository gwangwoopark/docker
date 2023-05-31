FROM node:18

ARG GIT_USER="Gwangwoo Park"
ARG GIT_EMAIL="gwangwoo.park@gmail.com"

USER root
RUN apt update && apt install -y zsh 
RUN apt install -y bat
RUN curl -sLf https://spacevim.org/install.sh | bash

USER node
RUN sh -c "$(curl -fsSL https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
RUN git clone --depth=1 https://github.com/romkatv/powerlevel10k.git ${ZSH_CUSTOM:-$HOME/.oh-my-zsh/custom}/themes/powerlevel10k
RUN git clone https://github.com/zsh-users/zsh-completions ${ZSH_CUSTOM:-${ZSH:-~/.oh-my-zsh}/custom}/plugins/zsh-completions
RUN git clone https://github.com/zsh-users/zsh-syntax-highlighting ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting
RUN git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions
RUN git clone https://github.com/zsh-users/zsh-history-substring-search ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-history-substring-search

USER node
RUN git config --global user.name ${GIT_USER} \
  && git config --global user.email ${GIT_EMAIL} \
  && git config --global core.editor "vim"

USER root
COPY .docker.zshrc /home/node/.zshrc
COPY .docker.p10k.zsh /home/node/.p10k.zsh
RUN chown node:node /home/node/.zshrc /home/node/.p10k.zsh

USER root
RUN apt install -y default-jdk

USER node
WORKDIR /home/node 
ENTRYPOINT [ "zsh" ]
