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
# Install nvim from source for arm64
RUN apt-get install -y ninja-build gettext cmake unzip
RUN git clone https://github.com/neovim/neovim
RUN cd neovim && make CMAKE_BUILD_TYPE=RelWithDebInfo
RUN cd neovim && make install

RUN sh -c 'curl -fLo "${XDG_DATA_HOME:-$HOME/.local/share}"/nvim/site/autoload/plug.vim --create-dirs \
       https://raw.githubusercontent.com/junegunn/vim-plug/master/plug.vim' 
RUN echo "colorscheme codedark" > ~/.config/nvim/setting/color.vim  
RUN cd ~ && rm -dRf neovim  
#RUN ln -s /opt/nvim-linux64/bin/nvim /usr/bin/nvim 
RUN npm install -g neovim 
RUN nvim --headless +PlugInstall +qall
RUN cd ~/.config/nvim/coc/extensions/ && npm install coc-pairs --only=prod
RUN git config --global user.email "melektaus2019@gmail.com" && git config --global user.name "Melxorn"

RUN exec bash
CMD ["/bin/bash"]
