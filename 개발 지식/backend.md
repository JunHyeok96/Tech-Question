# Backend 개발 지식

## 웹 개발 지식

<details>
    <summary style="font-size : 20px;"><strong> Q. 웹 서버와 WAS는 어떤 차이가 있나요?</strong></summary></br> 

웹 서버는 정적인 컨텐츠를 처리하며 WAS에 요청을 전달하고 WAS로부터 응답을 클라이언트에게 전달하는 역할을 수행합니다. ex)nginx, apache   

WAS는 웹 서버와 웹 컨테이너가 합쳐진 것으로 비즈니스 로직을 처리하여 동적인 컨텐츠를 제공할 수 있습니다. ex)tomcat, jetty, jboss    

웹 서버와 WAS를 구분하므로서 얻는 장점으로 웹 서버에서 정적인 컨텐츠를 담당하여 WAS에 대한 부하를 줄여줄 수 있으며 WAS에 요청을 전달할 때 로드밸런싱을 수행할 수 있습니다.
</details></br>

<details>
    <summary style="font-size : 20px;"><strong> Q. forward proxy와 reverse proxy는 무엇인가요?</strong></summary></br> 

forward proxy는 클라이언트가 인터넷에 직접 접근하는 것이 아니라 forward proxy 서버에서 요청을 받아 서버와 연결하고 결과를 응답하는 방식입니다.   

reverse proxy는 서버대신 proxy에서 요청을 받아 서버에게 요청을 전달해줍니다. 로드 밸런싱에 활용할 수 있고 서버를 노출하지않으므로서 보안상 이점이 있습니다.   

이 둘의 차이점은 아래와 같습니다.   
- forward proxy는 forward proxy server에서 서버로 요청을 보내므로 클라이언트를 감출 수 있고 reverse proxy는 서버 대신 reverse proxy서버에서 요청을 받아주므로 서버를 감출 수 있습니다.
- forward proxy의 endpoint는 실제 서버의 도메인인 반면, reverse proxy의 endpoint는 reverse proxy 서버 도메인입니다. 

</details></br>

<details>
    <summary style="font-size : 20px;"><strong> Q. 웹 페이지에서 어떻게 로그인 상태가 유지되나요?</strong></summary></br>

http는 stateless한 특징이 있어 이전 연결에 대한 결과를 저장하지 않습니다. 이런 단점을 극복하기 위해 http에서는 쿠키와 세션을 사용해 상태를 저장할 수 있습니다. 보통 클라이언트쪽에서 저장되는 쿠키는 보안상 취약하여 세션을 사용해 사용자의 정보를 서버측에서 관리합니다. 서버는 session id를 통해 사용자를 식별할 수 있으며 session id를 기준으로 로그인 정보를 저장할 수 있습니다. 클라이언트는 매 요청시 session id가 포함된 쿠키를 서버로 전송하므로 로그인 유지가 가능합니다. 

</details></br>

<details>
    <summary style="font-size : 20px;"><strong> Q. 톰캣이 재시작되면 로그인 정보가 날라가는 문제는 어떻게 해결해야하나요?</strong></summary></br>
  
세션 스토어를 메모리로 사용했다면 톰캣이 재시작시 로그인 정보가 날라가는 문제가 있습니다. 또한, 여러 서버로 운영을 할 때 메모리 상의 세션 정보를 공유하기 어렵습니다. 로드 밸런싱의 sticky session을 사용해서 세션이 저장된 동일한 서버에 요청을 보내는게 가능하지만 진정한 의미의 부하 분산이 이뤄지지 않습니다. 별도의 세션 스토어를 사용하면 서버가 셧다운되어도 세션에 대한 정보가 유지되고 클러스터링이 용이하다는 장점이 있습니다. 대표적인 세션 스토어로는 redis가 있습니다.
</details></br>

<details>
    <summary style="font-size : 20px;"><strong> Q. HttpSession은 어떻게 서로다른 session을 구분하나요?</strong></summary></br>
  
세션의 동작과정은 이렇습니다. 최초에 클라이언트의 요청이 들어오면 서버는 새로운 HttpSession객체를 생성하고 unique한 session id를 만듭니다. 응답에서 JSessionId를 key로 session id를 value로 하는 쿠키를 만들어 set-cookie 헤더에 담아 전달합니다. 이후 클라이언트는 session id정보가 포함된 쿠키를 요청에 담아 전송합니다. 서버의 서블릿 컨테이너는 모든 요청에 대해 쿠키로 담긴 JSessionId를 확인하여 저장된 HttpSession객체를 가져옵니다. 이 과정을 통해 클라이언트에 대한 정보를 식별할 수 있습니다.
</details></br>



## Servlet
<details>
    <summary style="font-size : 20px;"><strong> Q. servlet은 무엇인가요?</strong></summary></br>
    
java servlet은 클라이언트 요청에 대한 동적인 처리를 위해 사용되는 프로그램 혹은 사양을 말하며 servlet 컨테이너에 의해 실행되고 관리됩니다.
servlet의 life cycle은 init, service, destroy 3가지로 구분됩니다. 
- init은 서블릿이 최초의 한번만 실행되며 서블릿을 초기화합니다. 
- service는 doGet(), doPost()메서드를 가지고 있으며 클라이언트의 요청을 처리합니다. 
- destory는 servlet이 제거된 상태입니다. 


요청이 들어올 때마다 매번 servlet객체를 만드는 비용이 크기 때문에 init메서드가 호출되면 servlet은 메모리에 저장됩니다.
</details></br>

<details>
    <summary style="font-size : 20px;"><strong> Q. servlet container는 무엇인가요?</strong></summary></br>

servlet container는 http요청을 받아 servlet을 실행시키고 결과를 클라이언트에 응답하는 기능을 제공하는 컴포넌트입니다. servelt container의 역할은 다음과 같습니다.
- servlet을 실행하고 생명주기를 관리
- 멀티 스레딩을 관리하여 클라이언트의 여러 요청을 처리
- 웹 서버(nginx, apache..)와 통신을 지원  
대표적인 container로 tomcat, jetty, jboss등이 있습니다.
</details></br>

<details>
    <summary style="font-size : 20px;"><strong> Q. servlet 동작 과정은?</strong></summary></br>
    
1. 클라이언트가 요청을 전송하면 servlet container에서 HttpServletRequest, HttpServletResponse를 생성합니다.
2. web.xml파일을 확인하여 어떤 서블릿에 대한 요청인지 구분합니다.
3. 컨테이너는 service()메서드를 호출하여 요청 메서드에 따라 doGet() 또는 doPost()를 호출합니다.
4. doGet(), doPost()메서드에서 요청을 처리하여 HttpServletResponse객체에 응답을 전달합니다.
5. 응답이 완료되면 HttpServletRequest, HttpServletResponse객체는 소멸됩니다.
</details></br>


## JPA
<details>
    <summary style="font-size : 20px;"><strong> Q. ORM/JPA는 무엇인가요?</strong></summary></br>
    
ORM은 object relational mapping의 약자로 객체와 데이터베이스간 데이터를 매핑해주는 기법입니다.

JPA는 자바 ORM에 대한 API 표준 명세입니다. 대표적인 구현체로 hibernate가 있습니다.
</details></br>
    
<details>
    <summary style="font-size : 20px;"><strong> Q. JPA는 장단점이 무엇인가요?</strong></summary></br>
   
**장점**  
**생산성**  
JPA를 사용하면 자바 컬렉션에 객체를 저장하듯이 JPA에 저장할 객체를 전달하면됩니다. SQL을 작성하고 JDBC API를 사용하는 반복적인 일을 JPA에서 대신해준다. 

**유지보수**   
유지 보수 측면에서는 SQL의존적인 개발은 엔티티 컬럼이 변경되는 상황에서 연관된 모든 SQL문을 수정해야하고 결과를 매핑하기 위한 JDBC API도 변경해야합니다. JPA는 이러한 과정을 대신 처리해줍니다.

**패러다임 불일치 문제 해결**    
데이터 베이스는 데이터 중심으로 구조화되어있어 객체 지향 언어와 패러다임 불일치 문제가 발생합니다. JPA는 데이터 베이스에 맞춰 데이터를 저장하기위해
객체를 매핑하는 과정에서 드는 비용을 없애 좀 더 객체지향적인 개발이 가능하게 합니다.

**DB벤더 교체 용이성**   
SQL의존적인 개발은 DB벤더마다 문법이 다르기 때문에 다른 데이터베이스로 변경하는 작업이 쉽지않습니다. JPA는 데이터베이스와 애플리케이션 사이에서 추상화 계층을 제공하므로 변경 작업이 비교적 간편합니다,

**단점**
JPA의 단점은 복잡한 통계 쿼리를 처리하기 어려운 특성이있고, 실제로 어떤 쿼리가 실행될지 알 수 없으며, N+1문제, 등이 발생할 수 있습니다.
</details></br>

<details>
    <summary style="font-size : 20px;"><strong> Q. JPA에서 N+1문제는 무엇인가요?</strong></summary></br>
    
N+1문제는 다른 테이블을 Join해서 가져오지않고 일일히 select문을 통해 join된 결과를 만들어내는 문제입니다. 

**원인**   
N+1문제는 1:N관계에서 JPQL을 사용할 때 발생할 수 있습니다.

먼저, JPQL의 동작 과정은 아래와 같습니다.
1. JPQL을 호출하면 데이터베이스에 우선적으로 조회합니다.
2. 조회한 값을 영속성 컨텍스트에 저장합니다.
3. 영속성 컨텍스트에 조회할 때 이미 존재하는 데이터가 있다면 데이터를 버립니다.
JPQL은 이러한 동작 과정에서 연관 관계를 고려하지 않고 SQL문을 생성합니다. 이후 JPA에서 글로벌 fetch전략을 적용시킵니다. 
Eager모드가 적용되어 있다면 JPA에서는 즉시 연관 관계를 만들어주기위해 select문에 N번 실행됩니다.
Lazy모드는 당장 N+1문제가 발생하지 않지만 연관된 관계를 참조하려할 때 JPA에서는 연관 관계를 만들기위해 조회가 이뤄집니다. 
 
 **해결책**
 1. fetch join : fetch join은 즉시 연관 엔티티를 가져오도록 할 수 있습니다. SQL자체에서 연관 엔티티를 가져오도록 강제합니다. fetch join은 inner join을 사용해서 연관 관계를 만들어냅니다.
 2. @EntityGraph : EntityGraph를 사용하면 명시적으로 가져올 엔티티를 정할 수 있습니다. EntityGraph는 Outer join을 사용해서 연관 관계를 만들어냅니다.

 이 두가지 방법은 카테시안 곱이 발생해서 데이터의 중복이 발생합니다. 이를 해결하기위해 distinct로 조회하거나 자료구조로 set을 사용할 수 있습니다.
 
 3. batch size : OneToMany관계에 batch size를 설정해서 한번에 불러올 데이터 수를 지정할 수 있습니다. where in구문을 사용하기 때문에 join이 사용되지 않지만 N+1문제를 해결할 수 있습니다.

</details></br>

<details>
    <summary style="font-size : 20px;"><strong> Q. 영속성 컨텍스트는 무엇인가요?</strong></summary></br>
    
영속성 컨텍스트는 엔티티를 영구 저장하는 환경입니다. 엔티티 매니저로 엔티티를 저장하거나 조회하면 영속성 컨텍스트에서 엔티티를 보관하고 저장합니다.
영속성의 생명주기는 비영속, 영속, 준영속, 삭제로 4가지가 있습니다.   
**비영속 상태**는 엔티티의 객체는 생성되었지만 아직 저장하지 않아 영속성 컨텍스트나 데이터베이스와 관련 없는 상태를 말합니다.    
**영속 상태**는 영속성 컨텍스트가 관리하는 상태를 말합니다.   
**준영속 상태**는 영속성 상태인 엔티티를 더이상 영속성 컨텍스트에서 관리하지않는 상태입니다. 준영속상태는 1차캐시부터 쓰기 지연 저장소까지 해당 엔티티를 관리하기 위한 모든 정보가 제거됩니다.    
**삭제**는 엔티티가 영속성 컨텍스트와 데이터베이스에서 삭제된 상태입니다.       

영속성 컨텍스트는 엔티티의 식별자(@Id)를 통해 구분합니다. 따라서 영속 상태는 반드시 식별자가 있어야합니다. 
영속성이 데이터베이스와 동기화하는 과정은 보통 트랜잭션이 커밋되는 순간이며 이를 flush라고합니다.
영속성은 1차 캐시, 동일성 보장, 변경 감지, 쓰기 지연등의 장점이있습니다.  

**1차 캐시**  
영속화가 된 엔티티는 1차 캐시에 저장되어 관리되며 1차 캐시에 존재한다면 데이터 베이스를 조회하지 않고 조회가 가능하므로 성능상 이점이 있습니다. 동일한 엔티티를 여러번 조회해도 
1차 캐시에 있는 값을 반환하여 각각의 엔티티는 동일성을 보장합니다.  
**쓰기 지연**  
엔티티 매니저는 트랜잭션이 커밋되기 전까지 엔티티를 데이터베이스에 저장하지 않습니다. 내부 쿼리 저장소를 통해 insert SQL을 저장하며 트랜잭션을 커밋할 때 쿼리를 데이터 베이스로 전송합니다.
한번에 쿼리를 전달하므로써 성능상 이점을 얻을 수 있습니다.  
**변경 감지**  
영속성 컨텍스트는 영속상태의 엔티티의 변경을 추적합니다. JPA는 엔티티를 영속성 컨텍스트에 보관할 때 최초의 상태를 스냅샷합니다. 그리고 flush시점에서 스냅샷과 비교하여 변경사항이 감지되면
update 쿼리를 쓰기 지연 저장소에 추가하고 SQL을 데이터베이스로 보냅니다. 따라서 JPA에서는 별도의 update메서드가 존재하지 않습니다.

flush는 엔티티 매니저의 flush()메소드를 직접 호출하거나, 트랜잭션 커밋, JPQL을 실행하므로서 동작하게 할 수 있습니다.


</details></br>

<details>
    <summary style="font-size : 20px;"><strong> Q. JPA에서 즉시 로딩과 지연 로딩은 무엇인가요?</strong></summary></br>
    
JPA는 객체 그래프로 연관된 객체를 탐색합니다. 그러기 위해서는 엔티티에 객체 필드가 존재해야 합니다. 하지만, 사용하는 쿼리에 따라 연관 관계를 참조할 필요가 없을 수도 있습니다.
JPA가 지원하는 즉시로딩은 엔티티를 조회할 때 연관된 객체를 함께 조회하는 것을 말하며, 지연 로딩은 연관된 객체를 함께 조회하지 않고 사용하는 시점에서 조회하는 것을 말합니다.
</details></br>


<details>
    <summary style="font-size : 20px;"><strong> Q. JPA의 양방향 매핑 관계에서 잘못하면 무한루프가 발생할 가능성이 있는데 이런 원인은 무엇이고 해결책은 어떤 것인가요?</strong></summary></br>
    
이런 상황은 보통 엔티티를 JSON으로 변환하려는 상황에서 발생합니다. 회원, 팀이 양방향 관계에 놓여있다고 했을 때 회원 엔티티를 JSON으로 변환 한다고 가정해보겠습니다.
JSON으로 변환과정에서 팀의 엔티티까지 변환시키려고 할 것입니다. 그렇다면 팀에 속한 필드도 JSON으로 변환되는 과정이 필요한데, 
이때 팀에 속한 회원도 JSON으로 변환하려고합니다. 결국에는 팀에서는 회원을, 회원에서는 팀을 JSON으로 변환시키려는 과정에서 무한 루프가 발생합니다.
그래서 보통 dto를 만들고 JSON으로 변환할 데이터만 정의해서 그 dto로 JSON을 만듭니다. 이외에도 JSON라이브러리에서는 무한 루프에 빠지지 않게 하는 어노테이션이나 기능을 제공합니다.
</details></br>


<details>
    <summary style="font-size : 20px;"><strong> JPA의 mappedBy속성은 무엇인가요?</strong></summary></br>
    
먼저, 테이블에서 양방향 관계와 객체의 참조 관계는 차이가 있습니다. 테이블에서는 외래키를 사용하여 양쪽에서 조인을 할 수 있습니다. 하지만 객체는 단방향 참조를 사용합니다. 
양방향을 참조하는 상황에서 객체의 참조는 둘이지만 외래키는 하나인 상황이 발생합니다. 따라서 두 객체중 테이블의 외래키를 관리할 주인을 설정해줘야합니다. 
주인은 외래키의 등록, 수정, 읽기등이 자유롭지만 주인이 아닌 곳에서는 읽기만 가능합니다. mappedBy속성은 외래키를 관리하는 주인을 설정하는 속성입니다. 
주인이 아닌 경우 mappedBy속성을 사용해서 주인을 지정해야합니다.
</details></br>

<details>
    <summary style="font-size : 20px;"><strong> JPA에서 프록시는 무엇인가요?</strong></summary></br>
     
지연 로딩을 사용하기위해 JPA에서는 실제 엔티티 객체 대신 데이터베이스 조회를 지연할 수 있는 가짜 객체가 필요합니다. 이것을 프록시 객체라고 합니다. 
엔티티 매니저에서 getReference()메서드를 사용하면 프록시 객체를 얻을 수 있습니다. 이 프록시 객체는 엔티티의 상속을 통해 만들어진 것으로 실제 엔티티와 겉모습이 같습니다.
프록시 객체에 값을 얻기위해 호출하면 엔티티 정보를 얻어오기위한 DB조회를 조회해 엔티티를 가져옵니다. 이를 프록시 초기화라고합니다. DB조회로 영속성 컨텍스트에 저장된 엔티티를 
프록시 객체가 참조하면서 값을 반환합니다. 이런 원리로 지연 로딩이 가능합니다.
</details></br>

<details>
    <summary style="font-size : 20px;"><strong> 조회작업에서 @Transaction(read=true)를 사용하면 왜 성능이 빨리지나요?</strong></summary></br>

JPA에서는 read=true 속성이 설정되면 영속성 컨텍스트 flush가 발생하지 않습니다. Flush는 영속성 컨텐스트의 변경내용을 데이터 베이스와 동기화하는 작업을 말합니다.
</details></br>

## Spring
<details>
    <summary style="font-size : 20px;"><strong> IoC, DI는 무엇인가요?</strong></summary></br>

DI는 의존성 주입으로 객체를 외부에서 주입하는 방식을 말합니다. 외부에서 객체를 주입 받게되면객체 내부에서 객체를 생성하는 방식보다 유지 보수가 좋습니다. 의존하는 객체가 변경될 시 코드마다 일일히 찾아 수정하지 않아도 외부에서 주입하는 객체만 수정하면됩니다. 스프링의 컨테이너는 사용자가 직접 의존성 주입을 하는 대신 스프링 컨테이너에서 DI를해준다. 이를 제어가 역전되었다고해서 IOC라고 표현합니다.
</details></br>

<details>
    <summary style="font-size : 20px;"><strong> Bean은 무엇인가요?</strong></summary></br>

Spring 컨테이너에서 관리되는 객체를 말합니다. Bean은 xml파일이나 자바 configuration으로 선언할 수 있습니다. 일반적으로 Bean은 singleton으로 관리되며 Prototype으로 지정시 객체 호출시 매번 새롭게 생성됩니다.
</details></br>

<details>
    <summary style="font-size : 20px;"><strong>프록시 패턴은 무엇인가요?</strong></summary></br>

프록시 패턴은 클라이언트의 요청을 프록시 객체로 대신 받아주는 방식입니다.
</details></br>

<details>
    <summary style="font-size : 20px;"><strong>스프링 AOP는 무엇인가요?</strong></summary></br>

OOP의 경우 비즈니스 로직의 모듈화가 핵심이라면 AOP는 인프라, 시스템 관련 부가기능등을 모듈화하여 공통된 기능을 처리하는 방식입니다. AOP가 적용된 대표적인 예시는 @Transactional, @Cacheable이 있습니다.

**Target** : 부가기능을 부여할 대상  
**Aspect** : 관심사를 모듈화 한 것으로 어드바이스와 포인트 컷을 함께 갖고있습니다.  
**Advice** : 실질적으로 부가기능을 담은 구현체  
**JoinPoint** : 어드바이스가 적용될 위치입니다. 스프링에서는 메소드 조인포인트만 제공합니다.
**Point cut** : 부가기능이 적용될 대상을 선정하는 방법  
**Proxy** : 타겟을 감싸서 타겟의 요청을 대신 받아주는 랩핑 오브젝트.   
**Weaving** : 지정된 객체에 aspect를 적용해서 새로운 프록시 객체를 생성하는 과정  

스프링 AOP는 프록시 패턴 기반으로 합니다. 프록시 패턴은 대신 해준다는 개념으로 이해하면된다. 실제 aop가 적용된 객체의 클래스를 확인해보면 proxy가 붙은 객체로 사용됩니다.  
</details></br>

<details>
    <summary style="font-size : 20px;"><strong>스프링 AOP는 어떻게 동작하나요?</strong></summary></br>

스프링 aop는 프록시 패턴의 런타임 위빙 방식을 사용합니다.
스프링 aop는 JDK Dynamic Proxy, CGLIB 두 가지를 사용합니다.   

**JDK Dynamic Proxy** : Proxy Factory에게 타겟의 인터페이스 정보를 넘겨주면 타겟의 인터페이스를 상속한 Proxy 객체를 생성한다. Proxy 객체에서 invocationHandler를 구현하면되는데,  invoke 메서드를 오버라이딩하면서 부가기능을 구현할 수 있습니다. JDK Dynamic Proxy방식은 Java reflection을 사용해 target class의 method를 invoke하며, Advise대상이든 아니든 모든 method call마다 reflection invoke를 실시하므로 성능이 떨어집니다. JDK Dynamic Proxy방식은 interface가 반드시 필요합니다.

**CGLIB** :  CGLIB는 Enhancer라는 클래스를 바탕으로 프록시를 생성합니다. CGLIB Proxy는 Target Class를 상속받아 생성된다. CGLIB방식은 Interface가 필요하지 않습니다. 하지만 상속을 이용하는 만큼, final, private와 같이 overriding이 불가능한경우 사용할 수 없다. @Transactional, @Cacheable같은 어노테이션을 사용할 때 private이 안되는 이유가 이것 때문이다. methodInterceptor를 구현하면되는데, intercept 메서드를 오버라이딩하면서 부가기능을 구현한다. CGLIB방식은 메서드가 최초 호출될 때만 동적으로 Bytecode를 생성하고 다음 호출부터는 재사용하기 때문에 속도가 더 빠르다.

Spring에서 타겟 클래스의 인터페이스 구현 여부에 따라 방식이 달라집니다. Spring boot는 이와 상관없이 CGLIB방식이 default입니다.
</details></br>

## 기타
<details>
    <summary style="font-size : 20px;"><strong>1급 객체는 무엇인가요?</strong></summary></br>

1급 객체는 다음과 같은 조건을 만족하는 객체입니다.
- 변수나 데이터 구조에 할당할 수 있음
- 파라미터로 사용가능함
- return값으로 사용 가능함
- 비교 연산이 가능함
</details></br>

<details>
    <summary style="font-size : 20px;"><strong>함수형 프로그래밍은 무엇인가요?</strong></summary></br>

함수형 프로그래밍은 순수 함수, 즉 동일한 input에 대해 항상 동일한 output을 반환하며 함수의 실행으로 인한 프로그램 내 side effect가 없는 함수를 조합하고 공유 상태, 변경 가능한 데이터, side effect를 피하여 소프트웨어를 만드는 프로세스입니다. 가장 핵심이 되는건 불변의 관점입니다.

함수형 프로그래밍을 사용하면 output은 항상 input에 의존적이며 사이드 이펙트가 없으므로 테스트가 쉽고 예측하기 힘든 문제가 발생하지 않습니다. 또한, 최적화의 관점에서 동일 input에대한 output이 같으므로 캐싱이 가능해집니다. 함수형 프로그래밍에서는 변경가능한 상태를 배제하므로 동시성 문제가 발생하지않는다는 장점도 있습니다.

</details></br>

<details>
    <summary style="font-size : 20px;"><strong>1급 컬렉션은 무엇인가요?</strong></summary></br>

1급 컬렉션은 컬렉션을 wrapping하면서 그 외 다른 맴버 변수는 없는 것을 의미합니다. 1급 컬렉션의 장점은 다음과 같습니다.

**비즈니스에 종속적인 자료구조**  
로또 번호의 validation을 검증할 때 매번 서비스 로직에서 검증대신 조건에 만족하는 자료구조로서 활용할 수 있습니다.
  
**collection의 불변성을 보장**     
wrapper 클래스에서 컬렉션 값을 변경하는 메서드를 만들지 않으면 컬렉션 자체가 불변이됩니다
  
**상태와 행위를 한 곳에서 관리**   
컬렉션에는 값에 대한 정보만 있으며 이 값을 사용하는 로직은 다른 곳에있음. 일급 컬렉션으로 상태와 행위를 한 곳에서하면 중복 코드를 줄이고 효율적인 관리가능합니다.
  
**이름이 있는 컬렉션**    
컬렉션 변수에 이름을 붙이는 것이 아닌 클래스에 명명이 가능하므로 검색이 용이합니다. 개발팀/운영팀간 의사소통시 명확한 용어가 정의됩니다.
</details></br>

