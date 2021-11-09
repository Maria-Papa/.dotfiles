# Maria .dotfiles

For a long time, I was searching for a simple way to keep my configurations somewhere easy to get. 
When I stumbled across some **dotfiles repositories**, I knew that I needed to create my own!

I came across multiple ways to **save** my dotfiles (git bare, stow, install script), but it looks like I prefer the **git bare** solution.
I found out this very helpful video [Git Bare Repository - A Better Way To Manage Dotfiles](https://www.youtube.com/watch?v=tBoLDpTWVOM) and followed along!

## How I used what I learned

### Folders

First of all, I started by installing git and creating some folders to keep my code and dotfiles in.

```bash
cd ~
sudo apt install git -y
mkdir -p  code/.dotfiles
```

### SSH Keys

Go to project location and install npm.

```bash
cd code/.dotfiles
sudo apt install npm -y
```

Generate a new SSH Key and add it to ssh agent ([Generating a new SSH key and adding it to the ssh-agent](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent)).

```bash
npm install ssh-keygen
ssh-keygen -t ed25519 -C "1maria.papa@gmail.com"
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_ed25519
```

Copy and add the ssh key to GitHub SSH keys ([Adding a new SSH key to your GitHub account](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/adding-a-new-ssh-key-to-your-github-account)).

```bash
cat ~/.ssh/id_ed25519.pub
```

### Git Bare

As perfectly explained by the video, we need to create a git bare repository inside our dotfiles folder and then add an alias for config in our `/.bashrc` file.
I also added a new line with a comment (#Custom) to know where my custom code starts.

```bash
git init --bare $HOME/code/.dotfiles
grep -qF "#Custom" $HOME/.bashrc || echo -e "\n#Custom"  >> $HOME/.bashrc
grep -qF "alias config='/usr/bin/git --git-dir=$HOME/code/.dotfiles --work-tree=$HOME'" $HOME/.bashrc || echo "alias config='/usr/bin/git --git-dir=$HOME/code/.dotfiles --work-tree=$HOME'" >> $HOME/.bashrc
```

This command hides the files that I'm not interested in.

```bash
config config --local status.showUntrackedFiles no
```

### Git Initialization

Add remote repository.

```bash
config remote add origin git@github.com:Maria-Papa/.dotfiles.git
```

### First Git Push

Add `.bashrc` file, add commit message and push it to master. The first time the command `config push --set-upstream origin master` is needed, later we can use it as `config push`.

```bash
config add .bashrc
config commit -m "Add .bashrc"
config push --set-upstream origin master
```
