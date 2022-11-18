# deploy hyperledger fabric on kubernetes

``` bash
docker pull centos:latest
docker pull hyperledger/fabric-orderer:2.2
docker pull hyperledger/fabric-peer:2.2.0
docker pull hyperledger/fabric-tools:2.2.0
docker pull paragones/chaincode-marbles:1.0
```

``` 
# 创建一个名为 hyperledger 的 namespace
kubectl create namespace hyperledger
# 切换至 hyperledger namespace 
kubectl config set-context --current --namespace=hyperledger

cryptogen generate --config=crypto-config.yaml --output ./crypto-config

configtxgen -profile TwoOrgsOrdererGenesis -channelID devchan -outputBlock ./channel-artifacts/genesis.block


# 产生Channel 所需档案
configtxgen -profile TwoOrgsChannel -outputCreateChannelTx ./channel-artifacts/channel.tx -channelID "mychannel"
# 产生 Org1 使用的 channel 设定文件
configtxgen -profile TwoOrgsChannel -outputAnchorPeersUpdate ./channel-artifacts/org1Anchors.tx  -channelID "mychannel" -asOrg org1MSP
# 产生 Org2 使用的 channel 设定文件
configtxgen -profile TwoOrgsChannel -outputAnchorPeersUpdate ./channel-artifacts/org2Anchors.tx  -channelID "mychannel" -asOrg org2MSP


kubectl create -f /deploy-hyperledger-fabric-on-k8s/file-populate-bastion/
```