![image](https://github.com/mztacat/Allora-Network-Worker-node/assets/31314340/1c784684-8407-45ba-9d98-3a8de21517cd)# Allora-Network-Worker-node





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
![image](https://github.com/mztacat/Allora-Network-Worker-node/assets/31314340/40f78d47-cfdd-4bb7-b005-c3e2d956b8f3)



---------------------
## Add Network 
Visit: https://explorer.edgenet.allora.network/wallet/suggest
Add to Keplr 
![image](https://github.com/mztacat/Allora-Network-Worker-node/assets/31314340/f322211d-60c2-4ad0-9fb5-24cde756113d)


## Request Faucet 
Visit:   https://faucet.edgenet.allora.network/ 
Copy address from Keplr or the CMD [allo.......]
![image](https://github.com/mztacat/Allora-Network-Worker-node/assets/31314340/ae8c2348-36c3-4149-a5c8-dbf61c20a4ac)
![image](https://github.com/mztacat/Allora-Network-Worker-node/assets/31314340/ef51c12a-57ed-42e3-8c5c-cb964d9a37e4)




## Install Worker 
```
git clone https://github.com/allora-network/basic-coin-prediction-node
cd basic-coin-prediction-node

mkdir worker-data && mkdir head-data
```
![image](https://github.com/mztacat/Allora-Network-Worker-node/assets/31314340/ce147710-863e-4f1a-8c65-09c61fe16d66)

### Granting permission 
```
sudo chmod -R 777 worker-data
sudo chmod -R 777 head-data
```


### Create Head key 
```
sudo docker run -it --entrypoint=bash -v ./head-data:/data alloranetwork/allora-inference-base:latest -c "mkdir -p /data/keys && (cd /data/keys && allora-keys)"
```
![image](https://github.com/mztacat/Allora-Network-Worker-node/assets/31314340/903a7a48-d8c4-45e0-a08a-4d5375c595e0)



### Create Worker key 
```
sudo docker run -it --entrypoint=bash -v ./worker-data:/data alloranetwork/allora-inference-base:latest -c "mkdir -p /data/keys && (cd /data/keys && allora-keys)"
```

### Copy head ID 
```
cat head-data/keys/identity
```
![image](https://github.com/mztacat/Allora-Network-Worker-node/assets/31314340/23a6a877-5938-4b31-a66e-7b3fe07670aa)




# Connecting Allora Chain 
```
rm -rf docker-compose.yml && nano docker-compose.yml
```

## Paste content 
Copy and paste these content 
Replace ```head-id``` and ```wallet_seed_phrase```

```
version: '3'

services:
  inference:
    container_name: inference-basic-eth-pred
    build:
      context: .
    command: python -u /app/app.py
    ports:
      - "8000:8000"
    networks:
      eth-model-local:
        aliases:
          - inference
        ipv4_address: 172.22.0.4
    healthcheck:
      test: ["CMD", "curl", "-f", "http://localhost:8000/inference/ETH"]
      interval: 10s
      timeout: 5s
      retries: 12
    volumes:
      - ./inference-data:/app/data

  updater:
    container_name: updater-basic-eth-pred
    build: .
    environment:
      - INFERENCE_API_ADDRESS=http://inference:8000
    command: >
      sh -c "
      while true; do
        python -u /app/update_app.py;
        sleep 24h;
      done
      "
    depends_on:
      inference:
        condition: service_healthy
    networks:
      eth-model-local:
        aliases:
          - updater
        ipv4_address: 172.22.0.5

  worker:
    container_name: worker-basic-eth-pred
    environment:
      - INFERENCE_API_ADDRESS=http://inference:8000
      - HOME=/data
    build:
      context: .
      dockerfile: Dockerfile_b7s
    entrypoint:
      - "/bin/bash"
      - "-c"
      - |
        if [ ! -f /data/keys/priv.bin ]; then
          echo "Generating new private keys..."
          mkdir -p /data/keys
          cd /data/keys
          allora-keys
        fi
        # Change boot-nodes below to the key advertised by your head
        allora-node --role=worker --peer-db=/data/peerdb --function-db=/data/function-db \
          --runtime-path=/app/runtime --runtime-cli=bls-runtime --workspace=/data/workspace \
          --private-key=/data/keys/priv.bin --log-level=debug --port=9011 \
          --boot-nodes=/ip4/172.22.0.100/tcp/9010/p2p/head-id \
          --topic=1 \
          --allora-chain-key-name=testkey \
          --allora-chain-restore-mnemonic='WALLET_SEED_PHRASE' \
          --allora-node-rpc-address=https://allora-rpc.edgenet.allora.network/ \
          --allora-chain-topic-id=1
    volumes:
      - ./worker-data:/data
    working_dir: /data
    depends_on:
      - inference
      - head
    networks:
      eth-model-local:
        aliases:
          - worker
        ipv4_address: 172.22.0.10

  head:
    container_name: head-basic-eth-pred
    image: alloranetwork/allora-inference-base-head:latest
    environment:
      - HOME=/data
    entrypoint:
      - "/bin/bash"
      - "-c"
      - |
        if [ ! -f /data/keys/priv.bin ]; then
          echo "Generating new private keys..."
          mkdir -p /data/keys
          cd /data/keys
          allora-keys
        fi
        allora-node --role=head --peer-db=/data/peerdb --function-db=/data/function-db  \
          --runtime-path=/app/runtime --runtime-cli=bls-runtime --workspace=/data/workspace \
          --private-key=/data/keys/priv.bin --log-level=debug --port=9010 --rest-api=:6000
    ports:
      - "6000:6000"
    volumes:
      - ./head-data:/data
    working_dir: /data
    networks:
      eth-model-local:
        aliases:
          - head
        ipv4_address: 172.22.0.100


networks:
  eth-model-local:
    driver: bridge
    ipam:
      config:
        - subnet: 172.22.0.0/24

volumes:
  inference-data:
  worker-data:
  head-data:
```

![image](https://github.com/mztacat/Allora-Network-Worker-node/assets/31314340/9ebd2df2-6b86-4b15-be75-186e6ef570a2)


Proceed with ```CTRL X + Y,``` then ```ENTER```


## Employ Worker 
```
docker compose build
docker compose up -d
```

![image](https://github.com/mztacat/Allora-Network-Worker-node/assets/31314340/5b86e398-aefb-4fa1-8a2e-cac61c54420c)

Wait for it . .  ..  . . 



## Check running service
```
docker ps
```
![image](https://github.com/mztacat/Allora-Network-Worker-node/assets/31314340/a011fd82-229e-4402-84f3-2394b02f94eb)



## Replace ID with ```CONTAINER_ID```
```
docker logs -f CONTAINER_ID
```

e.g  docker logs -f 12c69e3c45db  for mine
![image](https://github.com/mztacat/Allora-Network-Worker-node/assets/31314340/c21f5948-b5fd-4214-b3da-02e85644013a)




# Looking up Worker Node:
```
curl --location 'http://localhost:6000/api/v1/functions/execute' \
--header 'Content-Type: application/json' \
--data '{
    "function_id": "bafybeigpiwl3o73zvvl6dxdqu7zqcub5mhg65jiky2xqb4rdhfmikswzqm",
    "method": "allora-inference-function.wasm",
    "parameters": null,
    "topic": "1",
    "config": {
        "env_vars": [
            {
                "name": "BLS_REQUEST_PATH",
                "value": "/api"
            },
            {
                "name": "ALLORA_ARG_PARAMS",
                "value": "ETH"
            }
        ],
        "number_of_nodes": -1,
        "timeout": 2
    }
}'
```


Here is my response: 
![image](https://github.com/mztacat/Allora-Network-Worker-node/assets/31314340/76c6920a-9208-484f-bdb4-6d6aecaeded4)




## Check update node 
```
curl http://localhost:8000/update
```

## Check Inference node 
```
curl http://localhost:8000/inference/ETH
```
![image](https://github.com/mztacat/Allora-Network-Worker-node/assets/31314340/604447a0-ae02-48b3-ba0f-78c9190332b8)

















