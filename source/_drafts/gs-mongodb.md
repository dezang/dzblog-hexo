---
title: mongo
---

```sh
$ brew services list | grep mongodb
$ brew services start mongodb
$ brew services stop mongodb
```

Notes
The default plist provided by homebrew stores the mongod configuration at /usr/local/etc/mongod.conf. This configuration specifies the dbpath to be /usr/local/var/mongodb instead of the default /data/db.


## mongo db 설치

```sh
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 0C49F3730359A14518585931BC711F9BA15703C6
echo "deb [ arch=amd64,arm64 ] http://repo.mongodb.org/apt/ubuntu xenial/mongodb-org/3.4 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-3.4.list
sudo apt-get update
sudo apt-get install -y mongodb-org
sudo service mongod start
tail -f /var/log/mongodb/mongod.log
```

참고 : https://docs.mongodb.com/manual/tutorial/install-mongodb-on-ubuntu/

## nodejs 및 npm 설치
```
curl -sL https://deb.nodesource.com/setup_4.x | sudo -E bash -
sudo apt-get install -y nodejs npm
```
참고 : https://nodejs.org/ko/download/package-manager/
