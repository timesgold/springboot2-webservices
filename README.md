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
            
* 2일차 ( 07. 15 Wed )
    * 3장 : SpringBoot 에서 JPA로 DB 다루기
        * RDBMS와 객체지향 프로그래밍 언어의 패러다임 불일치
            * RDBMS는 어떻게 데이터를 저장할지에 초점이 맞춰진 기술
            * 객체지향 프로그래밍 언어는 메시지를 기반으로 기능과 속성을 한 곳에서 관리하는 기술
            
        * JPA ( 자바 표준 ORM )
            * ORM ( Object Relational Mapping ) 은 객체를 매핑
            * RDBMS와 객체지향 프로그래밍 언어를 중간에서 패러다임 일치를 시켜주는 기술
            * JPA를 잘 사용할려면 객체지향 프로그래밍과 RDBMS를 둘 다 이해해야 함
            * iBatis, MyBatis 는 ORM이 아닌 SQL Mapper ( 쿼리를 매핑 )
            * 영속성 컨텍스트 : 엔티티의 영구 저장하는 환경

        * 더티 체킹
            * JPA의 엔티티 매니저가 활성화된 상태로 트랜잭션 안에서 DB의 데이터를 가져오면 이 데이터는 영속성 컨텍세트가 유지된 상태
            * 위의 상태에서 Entity 객체의 값만 변경하면 별도로 Update쿼리를 날릴 필요가 없음
            
        * Spring Data JPA
            * spring-boot-starter-data-jpa dependency 추가 필요
            * Spring Data JPA 내부에서 구현제 매핑을 지원
                * 구현제 교체의 용이성 : Hibernate 외에 다른 구현체로 쉽게 교체하기 위함
                * 저장소 교체의 용이성 : RDBMS 외에 다른 저장소로 쉽게 교체하기 위함
        
        * h2 : 인메모리 RDBMS
            * com.h2database:h2 dependency 추가 필요
            * 별도의 설치가 필요 없이 프로젝트 의존성만으로 관리
            * 메모리에서 실행되므로 애플리케이션을 재시작할 때마다 초기화되어 테스트 용도로 많이 사용
            * H2 웹 콘솔 화면 : http://localhost:8080/h2-console
            * jdbc URL : jdbc:h2:mem:testdb 
        
        * Domain 패키지
            * domain이란, 소프트웨어에 대한 요구사항 혹은 문제영역
            * 기존에 MyBatis와 비교하였을때 dao 패키지와 다르며, xml 에 쿼리 class 에 쿼리의 결과만 담던 일들을 모두 Domain Class에서 해결
            * 참고에 좋은 도서 ( DDD Start : 최범균 )
            
        * Spring Web Layer
            * Web Layer
                * 컨트롤러 (@Controller)와 JSP/Freemarker 등의 뷰 템플릿 영역
                * 필터(@Filter), 인터셉터, 컨트롤러 어드바이스(@ControllerAdvice) 등의 *외부 요청과 응답*에 대한 전반적인 영역
                
            * Service Layer
                * @Service에 사용되는 서비스 영역
                * Controller와 Dao의 중간 영역에 사용
                * @Transactional이 사용되어야 하는 영역
                * 트랜잭션과 도메인 간의 순서 보장
                
            * Repository Layer : DB와 같이 데이터 저장소에 접근하는 영역으로 Dao 영역
            * Dtos : 계층 간에 데이터 교환을 위한 객체
            * Domain Model
                * 도메인이라 불리는 개발 대상을 모든 사람이 동일한 관점에서 이해할 수 있고 공유할 수 있도록 단순환 시킨 것
                * 무조건 데이터베이스의 테이블과 관계가 있어야만 하는 것은 아님
                * VO 처럼 값 객체들도 이 영역에 해당
                * 비즈니스 처리를 담당해야 함
                
            * Date와 Calendar 클래스의 문제점
                * 불변(변경이 불가능한) 객체가 아니라서 멀티스레드 환경에서 언제든 문제가 발생
                * Calendar의 월(Month) 값 설계가 잘못됨
                * 참고 URL : [Java의 날짜와 시간 API](https://d2.naver.com/helloworld/645609) 
                
* Annotation 정리
    * Id : 해당 테이블의 PK 필드
    * GeneratedValue : PK 생성 규칙
    * NoArgsConstructor : 기본 생성자 자동 추가
    * Getter : 클래스 내 모든 필드의 Getter 메소드 자동 생성
    * Autowired : 생성자로 Bean을 주입 받는 방식 ( 권장하지 않음 )
    * RequiredArgsConstructor : final이 선언된 모든 필드를 인자값으로 하는 생성자를 생성 
    * WebMvcTest : JPA 기능이 작동하지 않음
    * SpringBootTest와 TestRestTemplate 을 사용해야 함
    * MapperedSuperclass : JPA Entity 클래스들이 해당 클래스를 상속할 경우 필드들도 칼럼으로 인식
    * EntityListeners : 클래스에 Auditing 기능을 포함
    * EnableJpaAuditing : JPA Auditing 활성화
    * CreatedDate : Entity 생성되어 저장될 때 시간이 자동 저장
    * LastModifiedDate : Entity의 값을 변경할 때 시간이 자동 저장
    
    * Entity
        * 테이블과 링크될 클래스이며, 해당 클래스에서는 절대 Setter 메소드를 만들지 않음
        * 필드 값 변경이 필요할 경우 기존의 Setter 방식이 아닌, 목적과 의도를 명확히 하는 메소드를 추가
        * 카멜케이스 이름을 언더스코어 네이밍(_)으로 테이블 이름을 매칭 (SalesMananger.java -> sales_manager table)
        * PK는 Long 타입의 Auto_increment를 추천
        * Request/Response 클래스로 사용해서는 안되며 Entity Class와 Controller에서 쓸 Dto는 분리하여 사용해야 함
    
    * Column : 클래스의 필드는 모두 칼럼이지만, 기본값 외에 추가로 변경이 필요한 옵션이 있으면 사용
        * 문자열 사이즈(length) 기본 값을 255 -> 500 
        * 타입(columnDefinition)을 TEXT로 변경하고 싶거나(ex. content) 등의 경우에 사용
    
    * Builder
        * 생성자 대신에 사용하며, 생성자 상단에 선언 시 생성자에 포함된 필드만 빌더에 포함
        * 어느 필드에 어느 값을 채워야할지 명확하게 인지
        
    * After
        * JUnit에서 단위 테스트가 끝날때 마다 수행되는 메소드를 지정
        * 배포 전 전체 테스트를 수행 시 테스트간 데이터 침범을 막기 위해 사용
    
    
    