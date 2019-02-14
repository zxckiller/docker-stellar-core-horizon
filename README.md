# Stellar Quickstart Docker Image - devHelper
*fork from [docker-stellar-core-horizon](https://github.com/stellar/docker-stellar-core-horizon)*

## 改动说明
主要改动是将stellar-core从docker 中拿出来，可以通过外部调试工具启动进行调试（vs2017等）
postgresql和horizon继续留在docker中
如果需要调试horizon，同理，将postgresql和core留在docker中，外部启动调试horizon

注意：
1. docker for windows现阶段没办法修改Mount目录的权限，无法使用Persistent mode ，需要使用docker for linux
https://github.com/docker/for-win/issues/39

2. 为了可以使docker中的horizon可以访问外部core，需要docker for windows 18.03以上版本
https://stackoverflow.com/questions/24319662/from-inside-of-a-docker-container-how-do-i-connect-to-the-localhost-of-the-mach

## 使用方法
### 打包dev镜像

```sh  
docker build -f .\Dockerfile.dev -t stellar/debug .
```
### 启动镜像

```sh  
docker run --rm -it -p "8000:8000" -p "5432:5432"  --name stellar-dev stellar/debug
```

注意：不要导出11626和11625两个端口，否则外部调试core会由于端口占用启动不了

### 运行脚本

```sh  
./start --testnet
```

出现 "Wait for debug stellar-core"字样时，启动外部调试的stellar-core

回车继续运行，启动horizon

### 调试功能
使用官方laboratry

https://www.stellar.org/laboratory/#explorer?resource=transactions&endpoint=all&network=custom&horizonURL=http%3A%2F%2F127.0.0.1%3A8000&networkPassphrase=Test%20SDF%20Network%20%3B%20September%202015

custom endpoint 需要配置成如下：
Horizon URL:  http://127.0.0.1:8000
Network Passphrase: 根据不同的网络类型配置。


或者启动本地laboratry
https://github.com/stellar/laboratory