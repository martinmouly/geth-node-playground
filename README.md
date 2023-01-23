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
![image](https://user-images.githubusercontent.com/19230666/214062788-0d3a518b-5b9b-4c67-8376-cc572ff1e669.png)


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
![image](https://user-images.githubusercontent.com/19230666/214063956-57a95f0a-2368-4143-b8df-ff105f6841d0.png)

## Run Geth as a service on Goerli

