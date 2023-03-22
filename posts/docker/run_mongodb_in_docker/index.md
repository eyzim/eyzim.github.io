# 在 Docker 裡面安裝 Mongo DB


這篇文章介紹了如何使用 Docker 來運行 MongoDB 是一種 NoSQL 資料庫服務。文章首先解釋了 Docker 的基本概念，如容器、映像和卷，然後展示了如何從 Docker Hub 上拉取 MongoDB 的官方映像，並在本地或雲端啟動一個 MongoDB 容器。

MongoDB 是一種 NoSQL 資料庫服務，可以儲存和處理非結構化的資料。

它用 JSON 的文件作為資料單位，不需要預定義的架構。

它具有高性能、高可用性和水平擴展性的特點，適合開發各種規模和類型的應用程式。

## apt 更新

確認 apt 版本並更新

```
sudo apt update && sudo apt upgrade -y
```

## 新增資料夾

開好資料夾並移動進入

```
mkdir mongo-docker
cd mongo-docker
```

## 尋找 docker image

尋找~~世界上~~現有的 docker image

```
docker search mongo
```

{{< image src="/images/run_mongodb_in_docker/search_mongo.png"  height="200"
          src_s="/images/run_mongodb_in_docker/search_mongo.png"
          src_l="/images/run_mongodb_in_docker/search_mongo.png"
alt="search all dockers of 'mongo'" caption="search all dockers of 'mongo'" linked="false">}}

## 下載 docker image

不要找自己麻煩，請直接 pull 官方的 image 謝謝

```
docker pull mongo:latest
```

## 檢查 image

```
docker images
```

## run image

讓 mongo 這個 image 跑起來

```
docker run -itd --name mongo -p 27017:27017 mongo --auth
```

## 進入 docker 裡的 mongo

```bash
docker exec -it mongo mongosh admin
```

新增連線 mongo 的帳號密碼

{{< image src="/images/run_mongodb_in_docker/enter_docker.png"  height="200"
          src_s="/images/run_mongodb_in_docker/enter_docker.png"
          src_l="/images/run_mongodb_in_docker/enter_docker.png"
alt="search all dockers of 'mongo'" caption="search all dockers of 'mongo'" linked="false">}}

## 新增 mongoDB 帳密

```
db.createUser({ user:'admin',pwd:'password',roles:[ { role:'userAdminAnyDatabase', db: 'admin'},"readWriteAnyDatabase"]});
```

## 驗證 mongoDB 帳密

```
db.auth('admin', 'password')
```

## 檢查 mongoDB

看最後一眼 db

```
show dbs
```

## 離開 docker 內部

離開這個 docker 裡的 mongoDB

```
exit
```

## 打開 27017 port

```
sudo ufw allow 27017
```

可以直接用 Windows PC 的 IP 進行遠端連線

### MongoDB compass

如果有下載 [MongoDB compass](https://www.mongodb.com/try/download/compass)，可以直接用輸入以下字串

```
mongodb://admin:password@<linux server IP>/
```

### telnet

```
telnet <linux server IP> 27017
```

## 重啟 mongo docker

```
docker-ps
```

{{< image src="/images/run_mongodb_in_docker/images_check.png"  height="200"
          src_s="/images/run_mongodb_in_docker/images_check.png"
          src_l="/images/run_mongodb_in_docker/images_check.png"
alt="check process of mongo docker" caption="check process of mongo docker" linked="false">}}

```
docker start <container id>
```

## 清除已經停止的 docker

```
docker container prune
```

