---
title: "springboot mybatis mysql gradle 버전"
date: 2020-11-15 15:04:00 -0600
tags: springboot mysql mariadb gradle 
---
springboot mybatis mysql gradle 설정

## 1.build.gradle  
```xml
dependencies {
	implementation 'org.springframework.boot:spring-boot-starter-web'
	implementation 'org.mybatis.spring.boot:mybatis-spring-boot-starter:2.1.3'
	developmentOnly 'org.springframework.boot:spring-boot-devtools'
	runtimeOnly 'mysql:mysql-connector-java'
	testImplementation('org.springframework.boot:spring-boot-starter-test') {
		exclude group: 'org.junit.vintage', module: 'junit-vintage-engine'
	}
}
```

## 2. application.properties -> application.yml 로 변경해봄 
```properties
spring:
  datasource:
    driver-class-name: com.mysql.cj.jdbc.Driver
    url: jdbc:mysql://localhost:3306/mydb?serverTimezone=UTC&characterEncoding=UTF-8
    username: root
    password: test

mybatis:
  type-aliases-package: myBoardg.model
  mapper-locations: mappers/**/*.xml 
```
## 나머지는 maven 버전과 동일

* 소스 : <https://github.com/Jn22L/springboot-mysql-gradle>

