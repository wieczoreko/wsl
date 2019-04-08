# wsl
Instruction for Installation and configuration for WSL

# Good to know:
- Windows is mounted in Linux under /mnt/c
- do not! change linux files from Windows --> things will get corrupt (see: https://devblogs.microsoft.com/commandline/do-not-change-linux-files-using-windows-apps-and-tools/)

# Brief instruction
 1) WSL Installation
 2) Install all Tools
 3) Dotfiles and Plugins
 4) SSH Configuration
 5) ...


# Step 1: Install WSL

Manual Download of Linux Distro: https://docs.microsoft.com/en-us/windows/wsl/install-manual
- Rename file to zip file
- extract it
- load ubuntu.exe (or similar)
- execute:
```
sudo umount /mnt/c
sudo mount -t drvfs C: /mnt/c -o metadata
```

## Step 1.1: Configure Bash on Windows

  1) Check Option:  `Verwendung von STRG + UMSCHALT +C/V zum Kopieren/Einfügen`  
  2) Under `Farben` change to preffered Color Style ( Iam Using https://github.com/carloscuesta/materialshell/tree/master/shell-color-themes)
  3) Font: Lucida Console
  4) Font Size: 14
  
  
## Step 1.2: Configure MobaXterm

  1) change [colors] section from MobaXterm.ini file
  2) import or create ur sessions
  


# Step 2: Install needed Tools

```
sudo apt-get update
sudo apt-get install curl git zsh python3-pip awscli google-cloud-sdk tmux

aws configure
gcloud init
```

## Step 2.1: zsh
##### oh-my-zsh:
`sh -c "$(curl -fsSL https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"`
- Source: https://github.com/robbyrussell/oh-my-zsh

##### zsh-syntax-highlighting:
`git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting`

- Note: Plugin needs to be activated in .zshrc
- Source: https://github.com/zsh-users/zsh-syntax-highlighting

##### Tmux Plugins:
`git clone https://github.com/tmux-plugins/tpm ~/.tmux/plugins/tpm`

- Source: https://github.com/tmux-plugins

##### fzf (fuzzy finder for history):
```
git clone --depth 1 https://github.com/junegunn/fzf.git ~/.fzf
~/.fzf/install
```
- Source: https://github.com/junegunn/fzf

##### Dircolors
```
wget https://raw.github.com/trapd00r/LS_COLORS/master/LS_COLORS -O $HOME/.dircolors
```
- Note: Needs to be activated in .zshrc with: `eval $(dircolors -b $HOME/.dircolors)`
- Source: https://github.com/trapd00r/LS_COLORS



# Step 3: Dotfiles
Currently using this dotfiles:
- .bashrc
- .zshrc
- .vimrc
- .tmuxconf

 Just copy all files to homedir.
 

# Step 4: SSH

There are multiple options to use SSH.
1) Windows Software like MobaXterm
2) through WSL with Windows Pageant
3) through WSL with Linux ssh-agent

 
## Step 4.1: Deploy SSH with Pageant 

1) Download https://github.com/vuori/weasel-pageant/releases 
2) Extract in a Windows Directory (not WSL)
3) add to .zshrc: 
`eval $(<location where you unpacked the zip>/weasel-pageant -r)`
  
Source: https://github.com/vuori/weasel-pageant


### Step 4.1.1 Use Pageant as autostart
1) Right-Click -> New Shortcut
2) Destination:
`"C:\Program Files\PuTTY\pageant.exe" ""Path_to_first_.ppk_file"" ""Path_to_second_.ppk_file"" `
3) Place Shortcut in Startup Folder
```
wir +r
shell:startup
```

## Step 4.2: Deploy SSH Keys from zsh
1) deploy private Keys to ~/.ssh/
`chmod 600 ~/.ssh/... `
2) enable ssh-agent plugin in .zshrc3
3) add this to .zshrc
```
#### SSH-Agent-Config
zstyle :omz:plugins:ssh-agent agent-forwarding on
zstyle :omz:plugins:ssh-agent identities id_rsa1 id_rsa2
```
