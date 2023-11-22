FROM debian

WORKDIR /root/

# Innit setting
RUN mkdir -p /root/.config/tmux/ && \
      echo "alias ls='ls --color'" >> /root/.bashrc && echo "alias tmux='tmux -2'" >> /root/.bashrc && \
      echo "set -g status-bg colour43" >> /root/.config/tmux/tmux.conf && \
      echo "set -g status-fg colour243" >> /root/.config/tmux/tmux.conf && \
      rm /etc/localtime && ln -s /usr/share/zoneinfo/Europe/Kiev /etc/localtime

# Install default programs
RUN apt update -y && apt install -y git wget curl build-essential tmux
WORKDIR /root/.config/
RUN git clone https://github.com/melafir/nvim.git
WORKDIR /root/
# Nodejs install
RUN curl -SLO https://deb.nodesource.com/nsolid_setup_deb.sh
RUN chmod 500 nsolid_setup_deb.sh 
RUN ./nsolid_setup_deb.sh 20
RUN rm nsolid_setup_deb.sh
RUN apt install nodejs -y

RUN wget https://github.com/neovim/neovim/releases/download/nightly/nvim-linux64.tar.gz && \
      tar xzf nvim-linux64.tar.gz -C /opt/ && \
      sh -c 'curl -fLo "${XDG_DATA_HOME:-$HOME/.local/share}"/nvim/site/autoload/plug.vim --create-dirs \
       https://raw.githubusercontent.com/junegunn/vim-plug/master/plug.vim' && \
      echo "colorscheme codedark" > ~/.config/nvim/setting/color.vim && \ 
      rm nvim-linux64.tar.gz && \
      ln -s /opt/nvim-linux64/bin/nvim /usr/bin/nvim && \
      npm install -g neovim && \
      nvim --headless +PlugInstall +qall
RUN cd ~/.config/nvim/coc/extensions/ && npm install coc-pairs coc-tabnine --only=prod
RUN git config --global user.email "melektaus2019@gmail.com" && git config --global user.name "Melxorn"

RUN exec bash
CMD ["/bin/bash"]
