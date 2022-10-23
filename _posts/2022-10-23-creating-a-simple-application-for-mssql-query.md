---
title: mssql query를 위한 간단한 애플리케이션 만들기
categories: [dev]
comments: true
tags: [docker, nodejs, mssql]
---

### 테스트용 간단한 애플리케이션 만들기
mssql에서 간단한 query를 실행해 보고 싶은데, 로컬에서는 접속이 안됩니다.<br/>
서버에 배포 가능하고, 소스 수정 및 실행이 가능한  app을 만들고자합니다.
<br/>
<br/>
npm 프로젝트를 만들고, mssql 패키지를 설치합니다.
```bash
$ npm init
$ npm install mssql
```
<br/>
**index.js**
<br/>
다음은 `select * from table` 를 수행하기 위한 code입니다.
```javascript
// mssql 연동
var sql = require('mssql');
var config = {
    user: '',
    password: '',
    server: '', // ip
    port: ,
    database: '',
    stream: true,
    options: {
      useUTC: false,
      dateFirst: 1,
      encrypt: false, // 오류 발생으로 추가 한 부분
      trustServerCertificate: true // 오류 발생으로 추가 한 부분
    },
}

sql.on('error', err => {
    console.error('error : ', err);
})

sql.connect(config, err => {
    const request = new sql.Request();
    request.stream = true;
    request.query('select * from table'); // 쿼리 실행


    let rowsToProcess = [];
    request.on('row', row => {
        rowsToProcess.push(row);
    })

    request.on('rowsaffected', rowCount => { // 결과 출력
        console.log('rowCount : ', rowCount)
        console.table(rowsToProcess);
    })

    request.on('done', () => {
        sql.close();
    })
})
```
<br/>
docker로 배포하기 위해 이미지로 만들어 줍니다.
<br/>
**.dockerignore**
```
node_modules
```

**Dockerfile**
```Dockerfile
FROM node:18.10.0

RUN mkdir /app
WORKDIR /app
COPY package*.json .
RUN npm config set strict-ssl false -g
RUN npm install

COPY . .
```

```bash
$ docker build -t app .
```
<br/>
서버에서 application을 실행하고, 필요에 따라 얼마든지 소스 수정이 가능합니다.<br/>
이런 식으로 다른 db들도 응용이 가능할 거 같습니다.
```bash
$ docker run -it app bash
$ node index.js
```