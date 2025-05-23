# ubuntu_customizations

*Customizações padrões pra deixar o sistema no jeito (testado com Ubuntu 25.04)*

1) Instalação e personalização do ZSH, substituindo o gnome-terminal
   
   Instalação do 'ZSH'
   
   ```shell
   sudo apt update && sudo apt upgrade -y && sudo apt install -y zsh
   ```
   
    Instalação do 'Oh My Zsh'
   
   ```shell
   sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
   ```
   
    Instalação de Plugins (syntax-highlighting e autosuggestions)
   
   ```shell
   git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting && \
   git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions
   ```
   
    Instalação do tema 'robertolins'
   
   ```shell
   mkdir -p "${ZSH_CUSTOM:-$HOME/.oh-my-zsh/custom}/themes" && cat << 'EOF' > "${ZSH_CUSTOM:-$HOME/.oh-my-zsh/custom}/themes/robertolins.zsh-theme"
   # robertolins.zsh-theme
   
   # Use with a dark background and 256-color terminal!
   # Meant for people that uses python, venvs and git.
   
   # You can set your computer name in the ~/.box-name file if you want.
   
   # Borrowing shamelessly from these oh-my-zsh themes:
   #   fino-time
   #
   
   function virtualenv_info {
       [ "$CONDA_DEFAULT_ENV" ] && echo "%{$FG[239]%}with%{$reset_color%} %{$fg_bold[white]%}$CONDA_DEFAULT_ENV%{$reset_color%}%{$FG[239]%} activated%{$reset_color%} "
       [ "$VIRTUAL_ENV" ] && echo "%{$FG[239]%}with%{$reset_color%} %{$fg_bold[white]%}\$(basename $VIRTUAL_ENV)%{$reset_color%}%{$FG[239]%} activated%{$reset_color%} "
       [ -z "$CONDA_DEFAULT_ENV" ] && [ -z "$VIRTUAL_ENV" ] && echo ""
   }
   
   function prompt_char {
       git branch >/dev/null 2>/dev/null && echo '⠠⠵' && return
       echo '○'
   }
   
   function box_name {
     local box="\${SHORT_HOST:-\$HOST}"
     [[ -f ~/.box-name ]] && box="\$(< ~/.box-name)"
     echo "\${box:gs/%/%%}"
   }
   
   PROMPT="╭─%{\$FG[040]%}%n%{\$reset_color%} %{\$FG[239]%}at%{\$reset_color%} %{\$FG[033]%}\$(box_name)%{\$reset_color%} %{\$FG[239]%}in%{\$reset_color%} %{\$terminfo[bold]\$FG[226]%}%~%{\$reset_color%} \$(virtualenv_info)\$(git_prompt_info) %{\$FG[239]%}on%{\$reset_color%} \$(date +'%d/%m/%Y') %{\$FG[239]%}at%{\$reset_color%} %*
   ╰─\$(prompt_char) "
   
   ZSH_THEME_GIT_PROMPT_PREFIX="%{\$FG[239]%}on branch%{\$reset_color%} %{\$fg[255]%}"
   ZSH_THEME_GIT_PROMPT_SUFFIX="%{\$reset_color%}"
   ZSH_THEME_GIT_PROMPT_DIRTY="%{\$FG[202]%}✘✘✘"
   ZSH_THEME_GIT_PROMPT_CLEAN="%{\$FG[040]%}✔"
   EOF
   ```
   
    Renomeie o antigo .zshrc e crie um novo, usando seu tema e os plugins instalados
   
   ```shell
   mv ~/.zshrc ~/.zshrc_old && cat << 'EOF' > ~/.zshrc
   # If you come from bash you might have to change your $PATH.
   # export PATH=$HOME/bin:$HOME/.local/bin:/usr/local/bin:$PATH
   
   # Path to your Oh My Zsh installation.
   export ZSH="$HOME/.oh-my-zsh"
   export VIRTUAL_ENV_DISABLE_PROMPT=1
   ZSH_AUTOSUGGEST_HIGHLIGHT_STYLE='fg=cyan'
   
   # Set name of the theme to load --- if set to "random", it will
   # load a random theme each time Oh My Zsh is loaded, in which case,
   # to know which specific one was loaded, run: echo $RANDOM_THEME
   # See https://github.com/ohmyzsh/ohmyzsh/wiki/Themes
   #ZSH_THEME="robbyrussell"
   ZSH_THEME="robertolins"
   
   # Set list of themes to pick from when loading at random
   # Setting this variable when ZSH_THEME=random will cause zsh to load
   # a theme from this variable instead of looking in $ZSH/themes/
   # If set to an empty array, this variable will have no effect.
   # ZSH_THEME_RANDOM_CANDIDATES=( "robbyrussell" "agnoster" )
   
   # Uncomment the following line to use case-sensitive completion.
   # CASE_SENSITIVE="true"
   
   # Uncomment the following line to use hyphen-insensitive completion.
   # Case-sensitive completion must be off. _ and - will be interchangeable.
   # HYPHEN_INSENSITIVE="true"
   
   # Uncomment one of the following lines to change the auto-update behavior
   # zstyle ':omz:update' mode disabled  # disable automatic updates
   # zstyle ':omz:update' mode auto      # update automatically without asking
   # zstyle ':omz:update' mode reminder  # just remind me to update when it's time
   
   # Uncomment the following line to change how often to auto-update (in days).
   # zstyle ':omz:update' frequency 13
   
   # Uncomment the following line if pasting URLs and other text is messed up.
   # DISABLE_MAGIC_FUNCTIONS="true"
   
   # Uncomment the following line to disable colors in ls.
   # DISABLE_LS_COLORS="true"
   
   # Uncomment the following line to disable auto-setting terminal title.
   # DISABLE_AUTO_TITLE="true"
   
   # Uncomment the following line to enable command auto-correction.
   # ENABLE_CORRECTION="true"
   
   # Uncomment the following line to display red dots whilst waiting for completion.
   # You can also set it to another string to have that shown instead of the default red dots.
   # e.g. COMPLETION_WAITING_DOTS="%F{yellow}waiting...%f"
   # Caution: this setting can cause issues with multiline prompts in zsh < 5.7.1 (see #5765)
   # COMPLETION_WAITING_DOTS="true"
   
   # Uncomment the following line if you want to disable marking untracked files
   # under VCS as dirty. This makes repository status check for large repositories
   # much, much faster.
   # DISABLE_UNTRACKED_FILES_DIRTY="true"
   
   # Uncomment the following line if you want to change the command execution time
   # stamp shown in the history command output.
   # You can set one of the optional three formats:
   # "mm/dd/yyyy"|"dd.mm.yyyy"|"yyyy-mm-dd"
   # or set a custom format using the strftime function format specifications,
   # see 'man strftime' for details.
   # HIST_STAMPS="mm/dd/yyyy"
   
   # Would you like to use another custom folder than $ZSH/custom?
   # ZSH_CUSTOM=/path/to/new-custom-folder
   
   # Which plugins would you like to load?
   # Standard plugins can be found in $ZSH/plugins/
   # Custom plugins may be added to $ZSH_CUSTOM/plugins/
   # Example format: plugins=(rails git textmate ruby lighthouse)
   # Add wisely, as too many plugins slow down shell startup.
   plugins=(git zsh-autosuggestions zsh-syntax-highlighting)
   
   source $ZSH/oh-my-zsh.sh
   
   # User configuration
   
   # export MANPATH="/usr/local/man:$MANPATH"
   
   # You may need to manually set your language environment
   # export LANG=en_US.UTF-8
   
   # Preferred editor for local and remote sessions
   # if [[ -n $SSH_CONNECTION ]]; then
   #   export EDITOR='vim'
   # else
   #   export EDITOR='nvim'
   # fi
   
   # Compilation flags
   # export ARCHFLAGS="-arch $(uname -m)"
   
   # Set personal aliases, overriding those provided by Oh My Zsh libs,
   # plugins, and themes. Aliases can be placed here, though Oh My Zsh
   # users are encouraged to define aliases within a top-level file in
   # the $ZSH_CUSTOM folder, with .zsh extension. Examples:
   # - $ZSH_CUSTOM/aliases.zsh
   # - $ZSH_CUSTOM/macos.zsh
   # For a full list of active aliases, run `alias`.
   #
   # Example aliases
   # alias zshconfig="mate ~/.zshrc"
   # alias ohmyzsh="mate ~/.oh-my-zsh"
   
   . "$HOME/.local/bin/env"
   EOF
   ```
   
    Habilite o zsh como terminal padrão (será necessário dar um logoff/logon)
   
   ```shell
   chsh -s $(which zsh)
   ```

2) Instalação do Docker Desktop
   
    Instalando dependências
   
   ```shell
   sudo apt update
   sudo apt install -y \
       apt-transport-https \
       ca-certificates \
       curl \
       gnupg \
       lsb-release \
       uidmap \
       dbus-user-session \
       fuse-overlayfs \
       libfuse2
   ```
   
    Baixar o Docker Desktop
   
   ```shell
   wget https://desktop.docker.com/linux/main/amd64/191279/docker-desktop-amd64.deb -O docker-desktop.deb 
   ```
   
    Instalar o Docker Desktop
   
   ```shell
   sudo apt install ./docker-desktop.deb
   ```

3) Instalação do GitHub Desktop
   
   Fazer o download
   
   ```shell
   wget https://github.com/shiftkey/desktop/releases/download/release-3.4.13-linux1/GitHubDesktop-linux-amd64-3.4.13-linux1.deb
   ```
   
   Instalar
   
   ```shell
   sudo apt install ./GitHubDesktop-linux-amd64-3.4.13-linux1.deb
   ```

4) Instalação do uv
   
   ```
   curl -LsSf https://astral.sh/uv/install.sh | sh  
   ```

5) Instalação do VSCode
   
   ```shell
   sudo snap install code --classic
   ```

6) Instalação do Chrome
   
    Fazer o download
   
   ```shell
   wget https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb
   ```
   
    Instalar
   
   ```shell
   sudo apt install ./google-chrome-stable_current_amd64.deb
   ```

7) Instalação do MarkText
   
   Fazer o download
   
   ```shell
   wget https://github.com/marktext/marktext/releases/latest/download/marktext-amd64.deb
   ```
   
    Instalar
   
   ```shell
   sudo apt install ./marktext-amd64.deb
   ```

8) Instalação do Gnome Shell Extensions
   
   Instalar
   
   ```shell
   sudo apt install gnome-shell-extensions gnome-browser-connector
   ```

9) Instalações de Extensões

        Acessar: https://extensions.gnome.org/

        Buscar extensões e instalar:
            Caffeine
            Dash2Dock Animated
            Just Perfection

10. Instalação do VLC
    
    ```shell
    sudo apt install vlc -y
    ```
