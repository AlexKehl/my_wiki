# Setting up a Linux machine

## Users

### Add user

```bash
sudo adduser username
```

### Add user to sudo group

```bash
sudo usermod -aG sudo username
```

## Packages

### Update package list

```bash
sudo apt update
```

### Configure zsh

1. Install zsh

```bash
sudo apt install zsh
```

2. Install oh-my-zsh

```bash
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```

3. Install fzf

```bash
git clone --depth 1 https://github.com/junegunn/fzf.git ~/.fzf
~/.fzf/install
```

4. Use vi-mode

In `~/.zshrc` add `vi-mode` to the plugins list.
```bash
plugins=(
        ...
        vi-mode
)
```

### Install docker

1. Set up Docker's apt repository.

```bash
# Add Docker's official GPG key:
sudo apt-get update
sudo apt-get install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

# Add the repository to Apt sources:
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update
```

2. Install Docker Packages.

```bash
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

3. Dont require sudo to run docker commands. Need to logout and login again.

```bash
sudo usermod -aG docker $USER
```

### Install docker-compose

The following command will download the 1.29.2 release and save the executable file at /usr/local/bin/docker-compose, which will make this software globally accessible as docker-compose:
```bash
sudo curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
```

Next, make the downloaded file executable:
```bash
sudo chmod +x /usr/local/bin/docker-compose
```

### Install nvm

```bash
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.7/install.sh | bash
```

Don't forget to add this to .zshrc

```bash
export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"  # This loads nvm
[ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"  # This loads nvm bash_completion
```
