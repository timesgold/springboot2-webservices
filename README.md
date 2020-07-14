# 책 실습 : 스프링 부트와 AWS로 혼자 구현하는 웹 서비스

##### IDE : IntelliJ

##### Language : JAVA

##### Framework : SRPING

##### Build : Gradle

* 1일차 ( 07. 14. Tue )
    * 1장 : SpringBoot Start
        * mavenCentral VS jcenter
            * 각종 의존성 (라이브러리)들을 어떤 원격 저장소에서 받을지를 정한다
            * mavenCentral 에서는 자신이 만든 라이브러리를 업로드하기 위해 많은 과정과 설정이 필요하다
            * jcenter는 mavenCentral의 문제점을 개선하여 간단하게 바꾸었다. 또한, mavenCentral에도 업로드 될 수 있도록 자동화를 할 수 있다
    
        * Gradle -> SpringBoot 프로젝트 생성
            * Gradle의 경우 아직까지는 4 버전을 사용한다. 아래와 같이 두 가지 다운그레이드 방법이 존재한다 
                1. gradlew wrapper --gradle-version 4.10.2
                2. gradle-wrapper.properties에서 distributionUrl의 값을 https\://services.gradle.org/distributions/gradle-***4.10.2***-bin.zip 변경
            * 'java, eclipse, org.springframework.boot, io.spring.dependency-management' 필수 plugins
            * .gitignore를 생성하여 git에 commit 시 제외할 경로를 작성한다
        
    * 2장 : SpringBoot 에서 테스트 코드 작성하기
        * TDD, Unit Test ( 단위테스트 )
        
        * JUnit 사용
        
        * Lombok 사용법
            * plugins 설치필요
            * 프로젝트별로 설정을 해주어야 한다
            * build.gradle에 compile('org.project.lombok:lombok')
            * IntelliJ settings에서 Enable annotation processing 체크
            
* 2일차
    * 3장 : SpringBoot 에서 JPA로 데이터베이스 다루기
            * JPA 
                * ORM ( Object Relational Mapping ) 은 객체를 매핑
                * iBatis, MyBatis 는 ORM이 아닌 SQL Mapper ( 쿼리를 매핑 )

