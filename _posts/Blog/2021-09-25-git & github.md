---
title:  "빌드관리도구" 
excerpt: "빌드관리도구"

categories:
  - 
tags:
  - [Gradle, Spring, Maven]

toc: true
toc_sticky: true
 
date: 2024-03-12
last_modified_at: 2024-03-12

---


# 🗂️ 1. 빌드 관리 도구

---

## 📌 빌드란?

- 소스코드 파일을 컴파일에서 실행할 수 있는 가공물로 변환하는 과정 또는 결과물
- 작성한 소스코드(Spring boot의 경우 .java 파일), 프로젝트에서 쓰인 각각의 파일 및 자원(.xml, jpa, jpg, prpoerties) 등을 JVM 이나 톰캣 등의 WAS 가 인식할 수 있도록 패키징하는 과정 및 결과물

ex) **Java 프로젝트** ⇒ 개발자가 작성한 [EX.java](http://EX.java) 와 여러가지 정적 파일 등의 resource 가 존재하는데, 빌드를 하게 되면 소스코드를 컴파일하여 .class 파일로 변환하고, resource를 .class가 참조할 수 있는 적절한 위치로 옮긴 후, META-INF와 MANIFEST.MF들을 하나로 압축하는 과정

## 📌 빌드 관리 도구란?

- 애플리케이션을 생성하면서 여러가지 **외부 라이브러리를 추가**할 때, 필요한 라이브러리들을 자동으로 관리
- 다음과 같은 작업을 수행
    - 종속성 다운로드 - **전처리(Preprocessing)**
    - 소스코드를 바이너리 코드로 **컴파일(Compile)**
    - 바이너리 코드를 **패키징(Packaging)**
    - **테스트** 실행**(Testing)**
    - 프로덕션 시스템에 **배포(Distribution)**

# 📕 2. Maven

---

## 📌 Maven이란 ?

- **Java 전용 프로젝트 관리 도구**로, Apache Ant의 대안으로 만들어짐
- Maven은 아파치 라이센스로 배포되는 **오픈 소스 소프트웨어**이다.

## 📌 Maven특징

1. **LifeCycle**: 정해진 Lifecycle(미리 정의한 빌드 순서)에 의하여 작업을 수행하며, 전반적인 프로젝트 관리 기능을 포함하고 있다.
2. **프로젝트 모델링**: Maven은 필요한 라이브러리를 `pom.xml`에 정의한다.
3. **플러그인을 통한 전역적인 재사용**: Maven은 빌드에 대한 대부분의 책임을 각 `플러그인`에 위임한다. 이러한 플러그인들은 `Maven저장소(Repository)`에 저장되어 진다.

## 📌 Maven 설정파일

### 👉 setting.xml

- 메이븐을 빌드할 때 의존 관계에 있는 라이브러리와 플러그인을 `중앙저장소→로컬저장소(개발자 PC)`로 다운로드하게 되어있다. 그리고 로컬저장소의 기본 위치는`USER_HOME/.m2/repository`인데, `setting.xml`를 통해 원하는 로컬 저장소의 경로를 지정 및 변경할 수 있다.
- 관련 링크 : https://living-only-today.tistory.com/62

### 👉 pom.xml

- root에 존재하는 xml파일로, 프로젝트마다 1개씩 가지고 있다. 필요한 라이브러리를 `pom.xml`에 정의하면, 해당 라이브러리 실행, 설치에 필요한 다른 라이브러리까지 관리하고 네트워크를 통해서 자동으로 다운받아준다.
- **POM의 주요 기능**

| 기능 | 설명 |
| --- | --- |
| 프로젝트 정보 | 프로젝트의 이름, 개발자 목록, 라이센스 |
| 빌드 설정 | 소스, 리소스, 라이프 사이클별 실행한 플러그인(goal)등 빌드와 관련된 설정 |
| 빌드 환경 | 사용자 환경 별로 달라질 수 있는 프로파일 정보 |
| POM연관 정보 | 의존 프로젝트(모듈), 상위 프로젝트, 포함하고 있는 하위 모듈 등 |

## 📌  Maven의 Lifecycle

!https://velog.velcdn.com/images%2Falicesykim95%2Fpost%2F314cfb70-b354-42a8-92a1-d197830515fe%2FMaven-Life-Cycle.png

| 단계 | 내용 |
| --- | --- |
| Clean | 빌드 시 생성되었던 파일들을 삭제하는 단계 |
| Validate | 프로젝트가 올바른지 확인하고 필요한 모든 정보를 사용할 수 있는지 확인하는 단계 |
| Compile | 프로젝트의 소스코드를 컴파일 하는 단계 |
| Test | 유닛(단위) 테스트를 수행 하는 단계(테스트 실패시 빌드 실패로 처리, 스킵 가능) |
| Pacakge | 실제 컴파일된 소스 코드와 리소스들을 jar, war 등등의 파일 등의 배포를 위한 패키지로 만 드는 단계 |
| Verify | 통합 테스트 결과에 대한 검사를 실행하여 품질 기준을 충족하는지 확인하는 단계 |
| Install | 패키지를 로컬 저장소에 설치하는 단계 |
| Site | 프로젝트 문서와 사이트 작성, 생성하는 단계 |
| Deploy | 만들어진 package를 원격 저장소에 release 하는 단계 |

# 📘 3. Gradle

---

## 📌 Gradle 이란 ?

- **Ant Builder**와 **Groovy Script**를 기반으로 구축되어 기존 Ant의 역할과 배포 스크립의 기능을 모두 사용가능하며 스프링부트와 안드로이드에서 사용된다
- 안드로이드 앱의 공식 빌드 시스템
- 빌드 속도가 Maven에 비해 10~100배 가량 빠름
- JAVA, C/C++M Python 등을 지원
- Groovy Script 언어로 구성되어 있기에 XML과 달리 변수선언, if, else, for 등의 로직이 구현가능하며, 간결하게 구성 가능하다.

## 📌 Gradle 특징

1. **가독성**이 좋다 : 코딩에 의한 간결한 정의가 가능하므로 가독성이 좋다.
2. **재사용**에 용이 : 설정 주입 방식(Configuration Injection)을 사용하므로 재사용에 용이하다.
3. **구조적**인 장점 : Build Script를 **`Groovy`**기반의 **`DSL(Domail Specific Language)`**를 사용하여 코드로서 설정 정보를 구성하므로 구조적인 장점이 있다.
4. **편리함** : Gradle 설치 없이 **`Gradle wrapper`**를 이용하여 빌드를 지원한다.
5. **멀티 프로젝트** : Gradle은 멀티 프로젝트 빌드를 지원하기 위해 설계된 빌드 관리 도구이다.
6. **지원**: Maven을 완전 지원한다.

## 📌 Gradle 설정 파일

### 👉 setting.gradle

- 프로젝트 구성을 설정할 때 작성하는 파일. 보통 프로젝트간의 의존성 및 서브프로젝트, 교차 프로젝트 같은 **멀티 프로젝트를 구성할 때 사용**한다. 단, 싱글 프로젝트의 경우에는 생략이 가능하다.

### 👉 build.gradle

- 간단하게, 빌드에 대한 모든 기능을 정의하는 파일. 프로젝트에서 사용하는 환경 설정, 빌드방법, 라이브러리 정보 등을 기술함으로서 빌드 및 프로젝트의 관리환경을 구성한다.

## 📌 Gradle의 LifeCycle

!https://velog.velcdn.com/images%2Falicesykim95%2Fpost%2F2d246e14-40e5-46bd-bf83-a392a58f8d11%2F1_jV3ibAK_WwFQcBLENrnKbg.png

!https://velog.velcdn.com/images%2Falicesykim95%2Fpost%2Fe62d50ca-66ce-42c1-a88e-0f0e62b13922%2F1_HbDa6AamqcwHygWlM8OTVQ.png

| 단계 | 내용 |
| --- | --- |
| 1) 초기화(initialization) | 빌드 대상 프로젝트를 결정하고 각각에 대한 Project객체를 생성한다. settings.gradle 파일에서 프로젝트 구성한다. |
| 2) 구성(Configuration) | 빌드 대상이 되는 모든 프로젝트의 빌드 스크립트를 실행한다. |
| 3) 실행(Execution) | 구성 단계에서 생성하고 설정된 프로젝트의 태스크 중에 실행 대상을 결정한다. gradle 명령행에서 지정한 태스크 이름 인자와 현재 디렉토리를 기반으로 태스크를 결정하여 선택된 태스크들을 실행한다. |

# 📙 4. Maven VS Gradle

---

## 📌 라이브러리 의존성 설정

### 👉 Maven(pom.xml)

```jsx
<?xml version="1.0" encoding="UTF-8"?>
	<project xmlns="http://maven.apache.org/POM/4.0.0" 
						xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"         
						xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">    
		<modelVersion>4.0.0</modelVersion>    
		<parent>        
				<groupId>org.springframework.boot</groupId>        
				<artifactId>spring-boot-starter-parent</artifactId>        
				<version>2.5.2</version>        
				<relativePath/> <!-- lookup parent from repository -->    
		</parent>    
		<groupId>com.example2</groupId>    
		<artifactId>demo-maven</artifactId>    
		<version>0.0.1-SNAPSHOT</version>    
		<name>demo-maven</name>    
		<description>Demo project for Spring Boot</description>    
		<properties>        
				<java.version>11</java.version>    
		</properties>    
		<dependencies>        
				<dependency>            
						<groupId>org.springframework.boot</groupId>            
						<artifactId>spring-boot-starter</artifactId>        
				</dependency>         
				<dependency>            
						<groupId>org.springframework.boot</groupId>            
						<artifactId>spring-boot-starter-test</artifactId>            
						<scope>test</scope>        
				</dependency>    
		</dependencies>     
		
		<build>        
				<plugins>            
						<plugin>                
								<groupId>org.springframework.boot</groupId>                
								<artifactId>spring-boot-maven-plugin</artifactId>            
						</plugin>        
				</plugins>    
		</build> 
</project>
```

### 👉 Gradle(build.gradle)

```groovy
plugins {    
		id 'org.springframework.boot' version '2.5.2'    
		id 'io.spring.dependency-management' version '1.0.11.RELEASE'    
		id 'java'
} 

group = 'com.example'
version = '0.0.1-SNAPSHOT'
sourceCompatibility = '11' 

repositories {    
		mavenCentral()
} 
dependencies {    
		implementation 'org.springframework.boot:spring-boot-starter'    
		testImplementation 'org.springframework.boot:spring-boot-starter-test'
} 
test {    
		useJUnitPlatform()
}

```

- **스크립트 길이와 가독성 면에서 gradle이 우세**
- 빌드와 테스트 실행 결과 gradle이 더 빠르다(**gradle은 캐시를 사용**하기 때문에 테스트 반복 시 차이가 더 커진다.)

### 👉 Gradle VS Maven 속도 비교

!https://blog.kakaocdn.net/dn/dKtmEU/btq8bsvQoKc/DjilAAylpcHLJFRtXQCd01/img.png

!https://blog.kakaocdn.net/dn/bS9riQ/btq8aN1zMjC/Kn1fpOCrvzF1lWKkoDNy4K/img.png

!https://blog.kakaocdn.net/dn/cEGqWA/btq8dulsWv5/RPudjkBsjzmp0gKd3Qk0z1/img.png

!https://blog.kakaocdn.net/dn/1me5I/btq8aOznGOG/MMblyASIa9QaGkmi9xEw30/img.png