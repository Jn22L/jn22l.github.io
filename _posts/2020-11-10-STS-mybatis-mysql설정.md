---
title: "스프링부트 mybatis mysql 최소한의 설정으로 끝내기"
date: 2020-11-10 17:47:00 -0600
categories: git github
---
* springboot mybatis mysql 최소한의 설정으로 끝내기
* 이젠 설정지옥에서 벗어나자 !

## 다운로드 및 설치 
1. STS4 다운 및 설치 : <https://spring.io/tools>
2. OpenJDK 다운 및 설치 : <https://jdk.java.net/>
3. MariaDb 다운 및 설치 : <https://downloads.mariadb.org/>
   - 테이블생성 해놓기 : mydb.mytable 컬럼은 id, name

## 스프링 기본셋팅 후 zip 파일 다운받기
* 여기서 : <https://start.spring.io/>
* 내가선택한것 : Maven Project/SpringBoot 2.3.5/OpenJdk 11  
*               Dependencies = Spring Web 만 선택
* 다운받기 클릭하면 -> zip 파일 생성해준다.

## 다운받은 zip 파일을 적당한 workspace 에 압축풀고 STS4 실행

## File > Import > Maven > Existing Maven Projects 선택 -> 압축푼 폴더 불러오기

## 설정파일 및 java 목록
최대한 심플하게 하기위하여, Service Layer 는 생략!
```
pom.xml
application.properties # DB설정
MyBoardApplication.java
model/BoardVO.java
BoardController.java
BoardMapper.java
boardMapper.xml
```

## 1. pom.xml dependency 추가
```XML
<dependency>
  <groupId>org.mybatis.spring.boot</groupId>
  <artifactId>mybatis-spring-boot-starter</artifactId>
  <version>2.0.0</version>
</dependency>

<dependency>
  <groupId>mysql</groupId>
  <artifactId>mysql-connector-java</artifactId>
</dependency>	
```

## 2. application.properties DB설정 추가
``` 
spring.datasource.driver-class-name= com.mysql.cj.jdbc.Driver
spring.datasource.url=jdbc:mysql://localhost:3306/mydb?serverTimezone=UTC&characterEncoding=UTF-8
spring.datasource.username=root
spring.datasource.password=test

# mybatis 매핑 type을 짧게 쓰기 위한 설정 
# mapper.xml에서 resultType을 지정할 때 myboard.model.BoardVO 대신 BoardVO로 간략히 할 수 있다. 
mybatis.type-aliases-package=myboard.model

# mapper.xml 위치 지정 
# **은 하위 폴더 레벨에 상관없이 모든 경로를 뜻하며, *는 와일드카드
# src/main/resources 아래에 mappers 폴더하위의 모든 xml 을 매퍼로 지정 한다는 의미
mybatis.mapper-locations=mappers/**/*.xml 
```

## 3. MyBoardApplication.java @MapperScan 어노테이션 추가 
```java
package myboard;

import org.mybatis.spring.annotation.MapperScan;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
@MapperScan("myboard.mapper") //이부분 추가! 이거 안해주면, Autowired 에러발생
public class MyboardApplication {
	public static void main(String[] args) {
		SpringApplication.run(MyboardApplication.class, args);
	}
}
```

## 4. VO 만들기 
```java
package myboard.model;
public class BoardVO {
	private int id;
	public int getId() {
		return id;
	}
	public void setId(int id) {
		this.id = id;
	}
	public String getName() {
		return name;
	}
	public void setName(String name) {
		this.name = name;
	}
	private String name;
}
``` 

## 5. controller 만들기 
```java
package myboard.controller;
 
import java.util.List;
 
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;
 
import myboard.model.BoardVO;
import myboard.mapper.BoardMapper;
 
@RestController
public class BoardController {
 
    @Autowired
    private BoardMapper boardMapper;
	    
    @RequestMapping("/selectAll")
    public List<BoardVO> selectAll() throws Exception{
        List<BoardVO> board = boardMapper.selectAll();
        return board;
    }
}
``` 

## 6. mapper 만들기 
```java
package myboard.mapper;
 
import java.util.List;
import myboard.model.BoardVO;

public interface BoardMapper {
    public List<BoardVO> selectAll()throws Exception;
}
``` 

## 7. 쿼리 xml 만들기 ( boardMapper.xml  )
위치 : src/main/resources 밑에 mappers 폴더생성후 작성
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd" >
 
<mapper namespace="myboard.mapper.BoardMapper">
    <select id="selectAll" resultType="BoardVO">
        SELECT * FROM MYTABLE
    </select>
</mapper>
``` 

## 실행해보기

* 프로젝트명 > Run As > Spring Boot App 선택
* 로컬주소 : http://localhost:8080/selectAll 
* mytable 의 모든 내용이 json 형태로 브라우저에 보이면 성공!

## 휴 ~ 이게 되네 ?

