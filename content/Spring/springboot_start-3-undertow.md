---
emoji: 👻
title: '[springboot_start] 내장 WAS를 undertow로 변경하기'
date: '2022-01-09 20:42:00'
author: chajin_ahn
tags: 
categories: Spring
---

이번 포스팅에서는 기존 WAS를 Tomcat에서 undertow로 변경해보겠습니다. tomcat은 오래전부터 사용된 WAS이나 아쉬운 점이 많아 사용 빈도가 줄고 있다고 합니다. 그래서 undertow로 바꿔보도록 하겠습니다.

## 라이브러리 가져오기

---

### 기존 Tomcat 라이브러리 제외하기

tomcat은 사용하지 않을 예정이므로 제외합니다.

```gradle
    configurations {
        all{
            // was tomcat 제외
            exclude module: 'spring-boot-starter-tomcat'
        }
    }
```

### undertow 라이브러리 추가

버전은 본인의 상황에 맞게 maven repository 에서 가져오시면 됩니다.
undertow 라이브러리와 autobulid 를 위한 devtools 를 추가합니다.
```gradle
    dependencies {
        implementation group: 'org.springframework.boot', name: 'spring-boot-starter-undertow', version: '2.6.3'
        developmentOnly 'org.springframework.boot:spring-boot-devtools'
    }
```

## application.yml 설정

### undertow + autobulid(devtools) 설정

```yaml
    ---
    spring:
      config:
        activate:
          on-profile: "common"
      devtools:
        livereload:
          enabled: true
        restart:
          enabled: true
    server:
      port: 8080
    ---
```
설정에 대한 프로파일 명을 __common__ 로 지정합니다. 해당 프로파일을 불러올 경우의 설정을 정의할 수 있습니다.

- spring.devtools.livereload.enabled=true: view 파일의 수정 사항을 즉각 반영합니다.
- spring.devtools.restart.enabled=true: java 파일의 수정 사항을 즉각 반영합니다.
- server.port=8080: 프로젝트의 포트를 지정합니다.

## 결과 확인

---

### 기존 Tomcat 환경

![tomcat](31-tomcat.png)

### undertow 환경

![undertow](32-undertow.png)

```toc
```
