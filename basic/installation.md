# 设置基本的fabric开发环境

## 构建docker镜像
### 下载源代码
```
    git clone https://github.com/hyperledger/fabric.git
```
### 构建docker镜像
```
    cd $GOPATH/src/github.com/hyperledger/fabric
    make images
```

## 启动fabric集群
### 启动fabric集群
使用docker-compose/security_pbft目录下的docker compose文件,启动一个fabric集群,该集群包含1个membersrvc结点和4个validator peer结点,使用pbft共识算法
```
    docker-compose up
```

## 执行智能合约
以example02为例, 部署/执行/查询智能合约:
### 登陆
```
    curl -H "Content-Type: application/json" -X POST --data '{"enrollId": "test_user0", "enrollSecret": "MS9qrN8hFjlE"}' -k http://localhost:7050/registrar
```
### 部署智能合约
```
    curl -i -X POST -H "Content-Type: application/json" http://localhost:7050/chaincode -d '{ "jsonrpc": "2.0", "method": "deploy", "params": { "type": 1, "chaincodeID":{ "path":"github.com/hyperledger/fabric/examples/chaincode/go/chaincode_example02" },"ctorMsg": { "args":["init", "a", "10000", "b", "0"] },"secureContext":"test_user0"}, "id": 5}'
```
### 执行智能合约
```
    curl -i -X POST -H "Content-Type: application/json" http://localhost:7050/chaincode -d '{ "jsonrpc": "2.0", "method": "invoke", "params": { "type": 1, "chaincodeID":{ "name":"d2ed7c063ea77f7d1be8ce370b6a9e8461ed37873d8631296fd085d0c3af72adc87012d90f4e856cf2746f9364440b539a662676a7e8e4385de641690583ef85" },"ctorMsg": { "args":["invoke", "a", "b", "1"] },"secureContext":"test_user0"}, "id": 6}'
```
### 查询智能合约
```
    curl -i -X POST -H "Content-Type: application/json" http://localhost:7050/chaincode -d '{ "jsonrpc": "2.0", "method": "query", "params": { "type": 1, "chaincodeID":{ "name":"d2ed7c063ea77f7d1be8ce370b6a9e8461ed37873d8631296fd085d0c3af72adc87012d90f4e856cf2746f9364440b539a662676a7e8e4385de641690583ef85" },"ctorMsg": { "args":["query", "a"] },"secureContext":"test_user0"}, "id": 7}'
```

## 其他
### Mac下环境准备
```
    brew install go rocksdb snappy gnu-tar
```
