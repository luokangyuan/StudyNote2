<p align="center">
<img width="130" align="center" src="http://image.luokangyuan.com/Java.svg"/>
</p>
<h1 align="center">开发工具—sonar</h1>

## 1.1.sonar简介

**代码质量管理工具**：`sonar`，sonar（SonarQube）是一个开源平台,用于管理源代码的质量，它不仅是一个质量数据报告工具，更是代码质量管理平台。它通过插件的形式来管理代码，它支持的语言包括：Java，PHP，C#，C等。

## 1.2.docker compose安装sonar

### 第一步：编写docker-compose.yml文件

```yaml
version: "3"

services:
  sonarqube:
    image: sonarqube
    ports:
      - "9000:9000"
    networks:
      - sonarnet
    environment:
      - SONARQUBE_JDBC_URL=jdbc:postgresql://db:5432/sonar
    volumes:
      - sonarqube_conf:/opt/sonarqube/conf
      - sonarqube_data:/opt/sonarqube/data
      - sonarqube_extensions:/opt/sonarqube/extensions
      - sonarqube_bundled-plugins:/opt/sonarqube/lib/bundled-plugins

  db:
    image: postgres:11.1
    networks:
      - sonarnet
    environment:
      - POSTGRES_USER=sonar
      - POSTGRES_PASSWORD=sonar
    volumes:
      - postgresql_data:/var/lib/postgresql/data

networks:
  sonarnet:
    driver: bridge

volumes:
  sonarqube_conf: 
  sonarqube_data: 
  sonarqube_extensions:
  sonarqube_bundled-plugins:
  postgresql_data:
```

### 第二步：运行docker-compose.yml

```bash
docker-compose up -d
```

### 第三步：访问并初始化

```properties
// 宿主机IP + 端口9000 登陆名 密码 默认都是admin
http://192.168.1.60:9000
```

### 第四步：分析项目

```bash
mvn sonar:sonar -Dsonar.host.url=http://192.168.1.60:9000 -Dsonar.login=326baf9a6cc3c7caa8815aac3eb894786cbca3c5 -Dsonar.login=admin -Dsonar.password=admin

```

### 页面预览

![image-20190825135658422](http://image.luokangyuan.com/2019-08-25-055705.png)

> PS: 有关`docker`和`componse`的安装请参考[ElasticSearch提高篇](http://luokangyuan.com/elasticsearch/)的附录，或者参考[docker学习笔记](<http://luokangyuan.com/dockerxue-xi-bi-ji/)