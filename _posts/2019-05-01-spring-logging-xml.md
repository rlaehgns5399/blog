---
title: "[Spring/Project] 스프링부트 logback 설정하기"
date: 2019-05-01 02:25:28 -0400
categories: Java/Spring
---



# What is logback?

SLF4J의 native 구현체. 왜 SLF4J를 함께 사용해야 하는지에 대한 내용은 [참조](<https://inyl.github.io/programming/2017/05/05/slf4j.html>)의 글을, 이 원리에 대한 내용은 [참조](<https://gmlwjd9405.github.io/2019/01/04/logging-with-slf4j.html>)의 글을 추천드립니다.

logback-core, hogback-classic, logback-access의 모듈로 구성

Maven dependency



```xml
<dependency>
    <groupId>net.rakugakibox.spring.boot</groupId>
    <artifactId>logback-access-spring-boot-starter</artifactId>
    <version>2.7.1</version>
    <scope>runtime</scope>
</dependency>
<dependencies>
    <dependency>
        <groupId>org.slf4j</groupId>
        <artifactId>slf4j-api</artifactId>
        <version>1.7.7</version>
    </dependency>
</dependencies>
```

# logback 설정파일

### 설정파일의 위치 및 종류

src/main/resources/ 아래에 위치한다.  Spring boot 에서는 logback.xml로 설정하면 스프링 부트에대한 설정전에 로그백 설정이 되므로 제어 할 수가 없다. 

따라서 logback-spring.xml을 이용하던지 property의 logging.config = classpath:logback-${spring.profiles.active}.xml을 통해 각 프로파일별로 logback 설정파일을 관리하도록 한다.


### logback의 설정 항목

**Level**
TRACE - DEBUG - INFO - WARN - ERROR 순으로 오른쪽으로 갈수록 높은레벨.
출력 레벨 이상의 로그만 출력한다.

**Appendar**

이벤트마다 로그를 기록하는 기능을 처리하는 객체. 로그의 출력위치, 출력 형식등을 설정한다. logback-core모듈에는 3가지 기본 Appender이 있다.

1. ConsoleAppender : 로그를 콘솔에 출력
2. FileAppender : 로그를 지정 파일에 기록
3. RollingFileAppender : FileAppender을 상속. 날짜와 용량등을 설정해서 패턴에 따라 로그가 각기 다른파일에 기록되게 할 수 있음.

Loback-classic 모듈을 이용하면 원격에 로그를 기록할 수도 있다.

**Logger**

실제 로그 기능을 수행하는 객체. 각 Logger마다 name을 통해 구분한다. 최상위 로거인 Root Logger를 설정하면 이를 계층적으로 활용 할 수 있다.

이에 대한 자세한 내용과 설정항목에 대한 더 자세한 내용은 [참조](<https://thinkwarelab.wordpress.com/2016/11/18/java%EC%97%90%EC%84%9C-logback%EC%9D%84-%EC%9D%B4%EC%9A%A9%ED%95%9C-%EB%A1%9C%EA%B9%85logging-%EC%82%AC%EC%9A%A9%EB%B2%95/>)에 자세히 나와있습니다.



**기록위치**

 

참조

1. [JAVA에서 LogBack을 이용한 로깅(logging) – 사용법](<https://thinkwarelab.wordpress.com/2016/11/18/java%EC%97%90%EC%84%9C-logback%EC%9D%84-%EC%9D%B4%EC%9A%A9%ED%95%9C-%EB%A1%9C%EA%B9%85logging-%EC%82%AC%EC%9A%A9%EB%B2%95/>)