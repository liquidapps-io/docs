Mainnet DSP Node
================

## Pre-reqs
### Ubuntu/Debian
```bash
sudo su -
curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.34.0/install.sh | bash
export NVM_DIR="${XDG_CONFIG_HOME/:-$HOME/.}nvm"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh" # This loads nvm
nvm install 10
nvm use 10
sudo apt install make cmake build-essential
```

### Centos/Fedora/AWS Linux:
```bash
sudo su -
curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.34.0/install.sh | bash
export NVM_DIR="${XDG_CONFIG_HOME/:-$HOME/.}nvm"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh" # This loads nvm
nvm install 10
nvm use 10
sudo yum install make cmake3
```


## Install
```bash
nvm use 10
npm install -g @liquidapps/dsp --unsafe-perm=true
```
## Configuration
```bash
setup-dsp
```

### Storage backend
## Custom subdomain

