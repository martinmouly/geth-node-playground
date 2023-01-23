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
![image](https://user-images.githubusercontent.com/19230666/214073549-091bb770-0230-443e-b2ad-492144506163.png)



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
![image](https://user-images.githubusercontent.com/19230666/214073651-fbb214c1-f413-4655-8488-75352f3b3901.png)

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
sudo systemctl status geth
```
![image](https://user-images.githubusercontent.com/19230666/214073786-b09ba723-d8d1-4deb-bfd9-3937348018ad.png)


## Run Lighthouse as a service on Goerli  
The process is the same to run lighthouse a service. The config file to run lighthouse as a service on Goerli is as follows : 
```
[Unit]
Description=Lighthouse Ethereum Client Beacon Node (Prater)
Wants=network-online.target
After=network-online.target

[Service]
Type=simple
Restart=always
RestartSec=5
ExecStart=/usr/local/bin/lighthouse bn \
    --network goerli \
    --datadir /var/lib/lighthouse \
    --http \
    --execution-endpoint http://127.0.0.1:8551 \
    --metrics \
    --validator-monitor-auto \
    --checkpoint-sync-url https://goerli.checkpoint-sync.ethdevops.io \
    --execution-jwt /var/lib/ethereum/jwttoken

[Install]
WantedBy=multi-user.target
```
After reloading daemon and starting service, the lighthouse should be running.
Check lighthouse service status 
```
sudo systemctl status lighthouse
```
![image](https://user-images.githubusercontent.com/19230666/214074152-d702de8e-f3a9-40dd-8f21-8ac51297be8f.png)

## Check service log journal
To check the complete logs of both services (ideal to check sync, or debugging in general) 
```
sudo journalctl -f -u geth
```
```
sudo journalctl -f -u lighthouse
```

## Interact with the node
Enter the Geth JavaScript console
```
cd ~
geth attach .ethereum/goerli/geth.ipc
```
![image](https://user-images.githubusercontent.com/19230666/214074790-6b9b2956-ebd4-40cc-a787-845193309332.png)

From the Geth JS console, we can retrieve data from the blockchain.  
For example, let's get the last block number
```
> eth.lastBlock
```





