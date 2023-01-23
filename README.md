# td4-geth-node

## Installing Geth  
Retrieve Geth from repo and install
```
sudo add-apt-repository -y ppa:ethereum/ethereum
sudo apt -y install geth
```
Check Geth was installed correctly
```
geth version
```


## Installing Lighthouse as our consensus client  
Retrieve from repo and uncompress binaries
```
wget https://github.com/sigp/lighthouse/releases/download/v3.2.1/lighthouse-v3.2.1-x86_64-unknown-linux-gnu.tar.gz
tar xvf lighthouse-v3.2.1-x86_64-unknown-linux-gnu.tar.gz
```  
Move to path
```
sudo cp ~/lighthouse /usr/local/bin
rm ~/lighthouse
```  
Check lighthouse was installed correctly
```
lighthouse --version
```

## Run Geth as a service on Goerli
Before creating config file, we need JWT token
```
sudo mkdir -p /var/lib/ethereum
openssl rand -hex 32 | tr -d "\n" | sudo tee /var/lib/ethereum/jwttoken
sudo chmod +r /var/lib/ethereum/jwttoken
```
Create geth service config file  
```
sudo nano /etc/systemd/system/geth.service
```  
Geth config file : 
```
[Unit]
Description=Go Ethereum Client - Geth (Goerli)
After=network.target
Wants=network.target

[Service]
User=administrateur1
Type=simple
Restart=always
RestartSec=5
TimeoutStopSec=180
ExecStart=/usr/local/bin/geth  --goerli --http --authrpc.jwtsecret=/var/lib/ethereum/jwttoken

[Install]
WantedBy=default.target
```
Reload daemon and start service
```
sudo systemctl daemon-reload
sudo systemctl start geth.service
```
Check service status
```
sudo systemctl status geth.service
```

## Run Lighthouse as a service on Goerli

## Open the RPC API to interact with your node
Enter the Geth JavaScript console
```
cd ~
geth attach .ethereum/goerli/geth.ipc
```



