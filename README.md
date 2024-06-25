# Allora-Network-Worker-node





## Install packages 
```
sudo apt update && sudo apt upgrade -y
```
![image](https://github.com/mztacat/Allora-Network-Worker-node/assets/31314340/1d7cd561-6b16-4fe0-89f5-e606f3d787d2)



## Install dependencies 
```
sudo apt install ca-certificates zlib1g-dev libncurses5-dev libgdbm-dev libnss3-dev curl git wget make jq build-essential pkg-config lsb-release libssl-dev libreadline-dev libffi-dev gcc screen unzip lz4 -y
```
![image](https://github.com/mztacat/Allora-Network-Worker-node/assets/31314340/00783786-cb7c-422a-a079-e25dfc721d24)



--------------
## Install python 
### Press ```y``` and proceed 
```
sudo apt install python3
python3 --version

sudo apt install python3-pip
pip3 --version
```
![image](https://github.com/mztacat/Allora-Network-Worker-node/assets/31314340/baa5caf5-f835-470a-942a-9098579ce02c)


## Install Docker 
```
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg

echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

sudo apt-get update
sudo apt-get install docker-ce docker-ce-cli containerd.io
docker version
```
![image](https://github.com/mztacat/Allora-Network-Worker-node/assets/31314340/31c0d459-37f1-49f8-9b03-c81e14b26588)



## Install Docer compose 
```
VER=$(curl -s https://api.github.com/repos/docker/compose/releases/latest | grep tag_name | cut -d '"' -f 4)

curl -L "https://github.com/docker/compose/releases/download/"$VER"/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose

chmod +x /usr/local/bin/docker-compose
docker-compose --version
```
![image](https://github.com/mztacat/Allora-Network-Worker-node/assets/31314340/7710bd4c-bbf6-4662-a3cb-a7e1f16c1029)

### Docker compose poermission 
```
sudo groupadd docker
sudo usermod -aG docker $USER
```


## Install GO 
```
cd $HOME && \
ver="1.21.3" && \
wget "https://golang.org/dl/go$ver.linux-amd64.tar.gz" && \
sudo rm -rf /usr/local/go && \
sudo tar -C /usr/local -xzf "go$ver.linux-amd64.tar.gz" && \
rm "go$ver.linux-amd64.tar.gz" && \
echo "export PATH=$PATH:/usr/local/go/bin:$HOME/go/bin" >> $HOME/.bash_profile && \
source $HOME/.bash_profile && \
go version
```
![image](https://github.com/mztacat/Allora-Network-Worker-node/assets/31314340/1af466f2-3a0d-4b97-89e8-f5e33cac8e4d)



## Install Allorad: Wallet 
```
git clone https://github.com/allora-network/allora-chain.git
cd allora-chain && make all
allorad version
```
![image](https://github.com/mztacat/Allora-Network-Worker-node/assets/31314340/2a3f318a-fdc2-417f-9128-3199edc745a0)



## Recover wallet with seed-phrase created previously 
### Enter ```bip39``` key [seedphrase] then the keyphrase enter a password, twice. 
```
allorad keys add testkey --recover
```
![image](https://github.com/mztacat/Allora-Network-Worker-node/assets/31314340/83bf56c9-2a1c-4c19-93f3-37771264254d)





























