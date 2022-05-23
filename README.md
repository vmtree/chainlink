# Chainlink Node Configuration
Everything related to deploying our Chainlink node using docker.

We followed the [Running a Chainlink Node](https://www.youtube.com/watch?v=DO3O6ZUtwbs) tutorial. 

Note: This is a development setup with security issues all over.  It was created solely for rapid deployment used for smoke testing.  This should not be used for production without an architecture overhaul and security hardening.

# Setup 
Tested on a VPS (4 Cores/8GB RAM/200GB SSD) which had a fresh install of Ubuntu 20.04.  The steps below will bring up a working Chainlink Node on the Ethereum Rinkeby testnet. Connect to the VPS via ssh and run the following commands. 

#### Install docker if its not installed
```
which docker >/dev/null && echo "docker is installed!" || echo "docker not installed" && curl -sSL https://get.docker.com/ | sh
```

#### Set docker permissions
```
sudo usermod -aG docker $USER
```

#### Restart shell
```
exec bash
```

#### Install pre-requisites
```
apt install docker-compose -y 
apt install git -y
```

#### Clone VMT git repository
```
git clone https://github.com/vmtree/chainlink-node.git
```

#### Change and create directories
```
cd chainlink-node/
mkdir ~/.chainlink
```

#### Set password and GUI credentials (use different values)
```
echo "passwordgoesHERE666###" > ~/.chainlink/.password
echo "admin@chain.link" > ~/.chainlink/.api
echo "passwordgoesHERE" >> ~/.chainlink/.api
```

#### Set environment variables (use different values)
```
echo "DB_USER=postgres
DB_PW=Password123
DB_NAME=postgresdb
LOG_LEVEL=debug
ETH_CHAIN_ID=4
LINK_CONTRACT_ADDRESS=0x01BE23585060835E02B77ef475b0Cc51aA1e0709
ETH_URL=wss://rinkeby.infura.io/ws/v3/APIKeyGoesHere
ETH_HTTP_URL=https://rinkeby.infura.io/v3/APIKeyGoesHere
MIN_OUTGOING_CONFIRMATIONS=2
CHAINLINK_TLS_PORT=0
SECURE_COOKIES=false
GAS_UPDATER_ENABLED=true
ALLOW_ORIGINS=*" > ~/.chainlink/.env
export $(grep -v '^#' ~/.chainlink/.env | xargs)
```

#### Bring up Chainlink runtime and postgres containers
```
docker-compose up -d
```


# Environment Directory
Some sample .env files which the current node solution is based on.

# Jobs Directory
Contains toml job specs of the external adapters.
