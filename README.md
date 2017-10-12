# 20170819Blackboard盧老師教學用線上黑板

1.盧老師附贈一本 盧老師在大學開授「虛擬貨幣與區塊鏈技術」的上課指定教科書：Mastering 
Bitcoin (精通比特幣)

此書有多種語言翻譯版本，英文版由O'Rilly出版社出版。 其他語言由作者依C.C授權免費開放給其他語言自由翻譯


2.請同學觀看以下影片：

 a.GCP免費帳號的申請與使用 https://youtu.be/a3oBHg5bUmI

 b.建立GCP上的虛擬機  https://youtu.be/oAqH3NzXP7I


3.查詢以下地址：1APtYTnTxVG1HEt9b7V4pyA4JDgwtDqjvV


4.實作四 200行程式碼的迷你區塊鏈 

實作-如何將兩個原本獨立的迷你區塊鏈連結成一個聯盟鏈   
https://youtu.be/BHlkT6-52QI 


2017/10/13 實作五 教學影片:
hyperledgerFabric-v0.6-01-IBM Bluemix上的Fabric試用範例. https://youtu.be/GFMOXGU1Ns0

hyperledgerFabric-v0.6-02-共識系統的觀念解說 (20:06) https://youtu.be/yl9Hagy2oO8

hyperledgerFabric-v0.6-03實作演練 利用腳本程序把四個節點的區塊鏈運行起來-01 (19:19) 
https://youtu.be/NPT44pSHrw8

hyperledgerFabric-v0.6-04-實作演練:利用腳本程序把四個節點的區塊鏈運行起來-02 (15:14) 
https://youtu.be/g461_a63WP0


```
sudo docker pull hyperledger/fabric-peer:x86_64-0.6.1-preview
sudo docker tag hyperledger/fabric-peer:x86_64-0.6.1-preview hyperledger/fabric-peer:latest     
sudo docker tag hyperledger/fabric-peer:x86_64-0.6.1-preview hyperledger/fabric-baseimage:latest
```


```
出錯就全刪的指令
sudo docker ps -aq|xargs sudo docker rm -f
```

```
sudo docker run --name=vp0 \
    -p 7050:7050 \
    -v /var/run/docker.sock:/var/run/docker.sock \
    -e CORE_VM_ENDPOINT=unix:///var/run/docker.sock \
    -e CORE_LOGGING_LEVEL=debug \
    -e CORE_PEER_ID=vp0 \
    -e CORE_PEER_NETWORKID=dev \
    -e CORE_PEER_VALIDATOR_CONSENSUS_PLUGIN=pbft \
    -e CORE_PEER_ADDRESSAUTODETECT=true \
    -e CORE_PBFT_GENERAL_N=4 \
    -e CORE_PBFT_GENERAL_MODE=batch \
    -e CORE_PBFT_GENERAL_TIMEOUT_REQUEST=10s \
    --rm hyperledger/fabric-peer:latest peer node start

sudo docker run --name=vp1 \
    -p 8050:7050 \
    -v /var/run/docker.sock:/var/run/docker.sock \
    -e CORE_VM_ENDPOINT=unix:///var/run/docker.sock \
    -e CORE_LOGGING_LEVEL=info \
    -e CORE_PEER_ID=vp1 \
    -e CORE_PEER_NETWORKID=dev \
    -e CORE_PEER_VALIDATOR_CONSENSUS_PLUGIN=pbft \
    -e CORE_PEER_ADDRESSAUTODETECT=true \
    -e CORE_PBFT_GENERAL_N=4 \
    -e CORE_PBFT_GENERAL_MODE=batch \
    -e CORE_PBFT_GENERAL_TIMEOUT_REQUEST=10s \
    -e CORE_PEER_DISCOVERY_ROOTNODE=172.17.0.2:7051 \
    --rm hyperledger/fabric-peer:latest peer node start

sudo docker run --name=vp2 \
    -p 9050:7050 \
    -v /var/run/docker.sock:/var/run/docker.sock \
    -e CORE_VM_ENDPOINT=unix:///var/run/docker.sock \
    -e CORE_LOGGING_LEVEL=info \
    -e CORE_PEER_ID=vp2 \
    -e CORE_PEER_NETWORKID=dev \
    -e CORE_PEER_VALIDATOR_CONSENSUS_PLUGIN=pbft \
    -e CORE_PEER_ADDRESSAUTODETECT=true \
    -e CORE_PBFT_GENERAL_N=4 \
    -e CORE_PBFT_GENERAL_MODE=batch \
    -e CORE_PBFT_GENERAL_TIMEOUT_REQUEST=10s \
    -e CORE_PEER_DISCOVERY_ROOTNODE=172.17.0.2:7051 \
    --rm hyperledger/fabric-peer:latest peer node start

sudo docker run --name=vp3 \
    -p 10050:7050 \
    -v /var/run/docker.sock:/var/run/docker.sock \
    -e CORE_VM_ENDPOINT=unix:///var/run/docker.sock \
    -e CORE_LOGGING_LEVEL=info \
    -e CORE_PEER_ID=vp3 \
    -e CORE_PEER_NETWORKID=dev \
    -e CORE_PEER_VALIDATOR_CONSENSUS_PLUGIN=pbft \
    -e CORE_PEER_ADDRESSAUTODETECT=true \
    -e CORE_PBFT_GENERAL_N=4 \
    -e CORE_PBFT_GENERAL_MODE=batch \
    -e CORE_PBFT_GENERAL_TIMEOUT_REQUEST=10s \
    -e CORE_PEER_DISCOVERY_ROOTNODE=172.17.0.2:7051 \
    --rm hyperledger/fabric-peer:latest peer node start
```
```
sudo docker exec -it vp2 bash

peer chaincode deploy \
-p github.com/hyperledger/fabric/examples/chaincode/go/chaincode_example02 \
-c '{"function":"init", "args": [ "a" , "100" , "b" , "200"]}'

chaincode_name=ee5b24a1f17c356dd5f6e37307922e39ddba12e5d2e203ed93401d7d05eb0dd194fb9070549c5dc31eb63f4e654dbd5a1d86cbb30c48e3ab1812590cd0f78539

peer chaincode query -n $chaincode_name -c '{"Function": "query", "Args": ["a"]}'
peer chaincode invoke -n $chaincode_name -c '{"Function": "invoke", "Args": ["a", "b", "35"]}'
peer chaincode query -n $chaincode_name -c '{"Function": "query", "Args": ["a"]}'

```
