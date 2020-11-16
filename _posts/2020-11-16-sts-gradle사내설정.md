---
title: "springboot gradle 내부망에서 셋팅"
date: 2020-11-16 11:44:00 -0600
tags: springboot mysql mariadb gradle 
---
집에서 잘되던 gradle 사무실로 가져오니 에러발생
수정할 부분 정리

## 1.settings.gradle
```properties
rootProject.name = 'springboot-mysql-gradle' # 프로젝트명 맞춰주기
pluginManagement.repositories {
    maven { url "http://nexus.내부망.com:8081/repository/gradle-m2/" }
}
```

## 2. build.gradle
```properties
repositories {
    maven { url "http://nexus.내부망.com:8081/repository/gradle-m2/" }
    maven { url "http://nexus.내부망.com:8081/repository/maven-central/" }
    maven { url "http://nexus.내부망.com:8081/repository/egovframe/" }
    maven { url "http://nexus.내부망.com:8081/repository/maven-public/" }
}
```
## 3. gradle-wrapper.properties
* 로컬로 gradle url 변경
```properties
    #distributionUrl=https\://services.gradle.org/distributions/gradle-6.6.1-bin.zip # 외부url 주석
     distributionUrl=dists/gradle-6.6.1-bin.zip # 로컬로 변경
```
* 변경된 폴더에 gradle 파일 붛여넣기
  * dists 폴더생성후 gradle-6.6.1-bin.zip 붙여넣기
