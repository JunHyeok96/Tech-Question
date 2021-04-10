# JAVA

<details>
    <summary style="font-size : 20px;"><strong>  Q. JAVA 8, 11버전의 특징은 무엇인가요? </strong></summary></br>
   
**Java8**
- 저장된 데이터를 처리하기위한 stream api가 추가되었습니다.
- 람다 표현식을 통해 함수형 프로그래밍이 가능합니다.
- Optional class의 등장으로 null값 처리를 간결하게 할 수 있습니다.
- 인터페이스에 default메서드가 추가되어 기본 동작을 정의할 수 있습니다.
- java.time패키지에 새로운 API가 등장했습니다(LocalDate, LocalDateTime등) 기존의 Calendar클래스는 월이 0부터 시작하고 불변 객체가 아니라는 단점이 있었습니다.
- default GC은 parallel GC입니다.

**Java11**
- Nest기반 접근 제어를 통해 논리적으로 같은 클래스를 분리된 클래스로 컴파일 할 수 있게 해줍니다. nestedmates간에는 서로 private 맴버에 접근할 수 있습니다.
- HttpClient가 standard로 지정되었습니다.
- 람다 파라미터로 var를 사용할 수 있게되었습니다.
- ZGC, Eplison이라는 새로운 GC가 추가되었습니다.
- default GC은 G1GC입니다.

</details></br>

<details>
    <summary style="font-size : 20px;"><strong>  Q. JDK, JRE, JVM의 차이는?   </strong></summary></br>
   
**JDK** : 자바 개발 도구의 약자로 JRE와 개발에 필요한 도구를 포함합니다. JRE + 개발 도구  
**JRE** : 자바 실행 환경의 약자로로 JRE는 JVM이 자바 프로그램을 실행시킬 때 필요한 라이브러리 파일과 기타 파일들을 가지고 있습니다. JVM + 시스템 라이브러리   
**JVM** : 자바 가상 머신의 약자로 자바 소스 코드를 컴파일하여 만든 바이트 코드를 실행할 수 있습니다.   
</details></br>

<details>
    <summary style="font-size : 20px;"><strong>  Q. JAVA의 실행과정을 말해주세요. </strong></summary></br>
   
자바 코드를 컴파일하면 바이트코드가 생성되고 JVM은 바이트 코드를 운영체제가 이해할 수 있는 기계어로 바꿔 실행시켜주는 역할을 합니다.
</details></br>

<details>
    <summary style="font-size : 20px;"><strong>  Q. JAVA는 인터프리터 언어인가요? 컴파일 언어인가요?   </strong></summary></br>
   
자바 코드를 컴파일하여 바이트 코드를 생성하고 JVM의 execution engine이 runtime data area에 적재된 바이트 코드를 기계어로 번역하여 실행하는 역할을 합니다. 따라서, 컴파일과 인터프리터가 동시에 작동하는 하이브리드 성향을 가지고 있습니다. 자바가 느리다고 하는 이유 중 하나가 이런 실행 방식과 연관이 있습니다. JIT 컴파일러를 사용하면 매번 기계어로 번역하지 않고 이전에 실행한 코드를 캐싱하여 재사용하기 때문에 예전의 자바 인터프리터 방식에 비해 더 빠른 실행이 가능합니다.
</details></br>

<details>
    <summary style="font-size : 20px;"><strong>  Q. JVM 구조에 대해서 설명해주세요. </strong></summary></br>
   
JVM은 class loader, execution engine, garbage collector, runtime data area로 구성됩니다.  
- Class loader는 런타임시 class파일을 읽어 runtime data area의 메서드 영역에 적재하는 역할을 합니다.  
- Execution engine은 Runtime Data Area의 메서드 영역에 적재된 바이트 코드를 기계어로 변경해서 실행하는 역할을 합니다.  
- Garbage collector는 heap 메모리에 생성된 객체 중 참조되지 않는 객체들을 탐색 후 제거하는 역할을 한다. GC는 데몬 스레드로 수행되며, 수행 중에는 모든 스레드가 중단됩니다.    
- Runtime Data Area는 JVM의 메모리 영역으로서 자바 애플리케이션이 실행될 때 사용되는 데이터들을 적재하는 영역입니다. 이 영역은 메소드 영역, 힙 영역, 스택 영역, pc레지스터, 네이티브 메서드 스택으로 구분됩니다.
</details></br>

<details>
    <summary style="font-size : 20px;"><strong>  Q. JVM의 메모리 구조는 어떻게 구성되어 있나요? </strong></summary></br>
   
JVM의 메모리 구조는 메서드(클래스/static) 영역, 힙 영역, 스택 영역, pc 레지스터, 네이티브 메서드 스택으로 구성됩니다. 
- 메서드 영역에서는 프로그램이 실행중에 클래스가 사용되면 JVM은 해당 클래스의 class파일을 읽어 클래스에 대한 정보를 저장하는 영역입니다. static 맴버, static 메서드도 이 영역에 저장됩니다.
- 힙 영역은 new 키워드로 생성된 객체와 배열이 저장되는 영역이다. 메소드 영역에 로드된 클래스만 생성 가능합니다. GC에의해 참조되지 않는 메모리가 제거됩니다. 
- 스택 영역은 지역 변수, 매개 변수, 리턴 값등이 생성되는 영역입니다. 
- PC 레지스터는 현재 스레드가 실행되는 부분의 주소와 명령을 저장하는 영역입니다. 
- 네이티브 메서드 스택은 자바 외 언어로 작성된 네이티브를 위한 메모리 영역입니다.

스레드가 생성되면 메서드 영역과 힙 영역을 공유하고 스택 영역, pc 레지스터, 네이티브 메서드 스택은 새롭게 생성됩니다.
</details></br>

<details>
    <summary style="font-size : 20px;"><strong>  Q. GC는 어떻게 동작하나요? </strong></summary></br>
   
JVM의 힙 영역은 eden, survivor1, survivor2, old로 구성됩니다. GC는 마이너 GC와 메이저 GC로 나뉘어집니다.  
  
Minor GC는 Young Gerneration영역에서 일어나는 GC입니다. Young Gerneration영역은 eden, survivor영역을 말합니다.
1. 최초에 객체는 eden영역에 생성됩니다. eden영역이 가득차면 첫 번째 GC가 발생합니다. 참조되고 있지 않은 객체는 제거하고, 참조되고있는 객체는 survivor1영역에 eden영역의 메모리를 복사합니다.
2. eden영역이 다시 가득차면 eden영역에 객체와 survivor1영역에 생성된 객체중 참조되고있는 객체가 있는지 검사합니다. 참조되지 않는 객체는 제거하고 참조되는 객체는 survivor2영역에 복사합니다. 
3. survivor1과 survivor2로 객체의 이동을 반복하면서 살아있는 객체는 Age bit에 살아남은 횟수를 기록하고 age bit가 threshold값 이상이 되거나 survivor영역의 메모리가 부족해지면 old영역으로 이동합니다. survivor1과 survivor2를 이동하는 이유는 메모리 단편화를 피하기 위해서입니다. 이처럼 JVM에서 한 곳에 객체를 모으는 방식을 Compaction 이라고 합니다. 

Major GC는 Full GC로 Old 영역에서 일어나는 GC이다.
1.	Old영역에 데이터가 가득차면 GC를 실행합니다. old영역에 모든 객체를 검사하여 참조되고 있는지 확인합니다.
2.	참조되지 않는 객체들을 모아 한번에 제거한다. Minor GC에 비해 시간이 오래걸리고 작업중 GC스레드를 제외한 모든 스레드가 중단된다.
  </details></br>

<details>
    <summary style="font-size : 20px;"><strong>  Q. JAVA언어의 장단점은 무엇인가요? </strong></summary></br>
 
**장점**  
JVM에서 동작하므로 특정 운영체제에 종속되지않습니다.    
객체 지향언어로서 캡슐화, 상속, 다형성등을 지원합니다.   
Garbage Collector에 의해 사용하지 않는 메모리를 자동으로 수거합니다.   
멀티 스레딩이 가능합니다.   

**단점**  
바이트 코드로 컴파일 후 인터프리터 방식으로 동작하여 실행 속도가 느립니다.
checked exception은 예외 처리가 없다면 실행할 수 없습니다.
</details></br>
    
<details>
    <summary style="font-size : 20px;"><strong>  Q. JAVA의 접근 제어자에 대해 설명해주세요. </strong></summary></br>
 
private : 해당 클래스에서만 접근 가능합니다.     
package private(default) : 같은 package에서만 접근 가능합니다.  
protected : 같은 package와 상속 받은 하위 클래스에서 접근 가능합니다.   
public : 모든 클래스에서 접근이 가능합니다.    
</details></br>
            
<details>
    <summary style="font-size : 20px;"><strong>  Q. JAVA에서 static은 무엇인가요? </strong></summary></br>
 
static으로 선언된 필드와 메서드는 객체의 생성 없이도 접근이 가능합니다. static으로 선언된 맴버와 메서드는 클래스 로딩시 메서드 영역에 생성되고 프로그램이 종료될 때 소멸합니다. static으로 선언된 필드는 동일 클래스를 새롭게 생성하더라도 같은 값을 공유해서 사용하는 특징이있습니다. static으로 선언된 메서드에서는 클래스의 필드를 사용하지 못하고 static으로 선언된 필드만 사용이 가능합니다.
</details></br>

            
<details>
    <summary style="font-size : 20px;"><strong>  Q. static을 어떤 경우에 설정하면 좋을 까요? </strong></summary></br>
    
static은 클래스 로딩시 메소드 영역에 적재되고 프로그램 종료시 소멸하는 특징이 있습니다. static은 객체 생성 없이 사용할 수 있어 빠르지만 한번 만들어지면 GC에의해 제거되지 않기 때문에 너무 static을 남발하면 시스템 성능 저하를 가져올 수 있습니다. 또한, static 맴버는 값을 공유하는 특징이 있어 thread safe여부를 신경써야합니다. static으로 활용하면 좋은 상황은 객체의 생성 없이 접근 가능하게 유틸 클래스를 private 생성자로 구성하고 static 메서드를 사용하게 하는 방식이 있습니다. java에서 Math클래스가 이런 방식을 사용합니다. 또한, 싱글턴 패턴을 구현하는데 정적 팩터리 메서드를 만들어 동일한 인스턴스를 반환하도록 사용할 수 있고 인스턴스간 공유 데이터를 사용할 때 static 필드를 활용할 수 있습니다.

</details></br>

<details>
    <summary style="font-size : 20px;"><strong>  Q. Java에서 Generic은 무엇인가요?  </strong></summary></br>
    
Generic은 객체의 생성 시점에 타입을 결정하여 유연한 개발을 하는데 도움을 줍니다. 대표적으로 Collection 프레임워크가 Generic을 사용합니다. Generic없이도 Object타입으로 객체를 받아 처리할 수는 있지만, 런타임 과정에서 예기치 않은 ClassCastException이 발생할 수 있습니다. Generic은 이런 단점을 해소하기 위해 컴파일시 타입 체킹을 합니다. Object로 타입이라면 강제 casting이 필요하지만, Generic을 사용하면 컴파일러에서 캐스팅 코드를 생성해줍니다.

</details></br>

<details>
    <summary style="font-size : 20px;"><strong> Q. JAVA에서 오버로딩과 오버라이딩을 설명해주세요. </strong></summary></br>
      
오버로딩은 메서드의 이름은 같지만 매개 변수 형식이 다른 경우를 말합니다.
오버라이딩은 상위 클래스의 메서드를 하위 클래스에서 재정의하는 것을 말합니다.

</details></br>

<details>
    <summary style="font-size : 20px;"><strong> Q. JAVA는 Call by reference와 Call by value중 어느 것인가요? </strong></summary></br>
      
Java는 항상 call by value로 동작합니다. call by value임에도 불구하고 호출되는 함수에서 객체 값을 변경할 수 있는 이유는 변수의 레퍼런스를 넘기는 것이 아니라 변수의 값(인스턴스의 메모리 주소)을 복사해서 넘기기 때문입니다. 그런 이유로 호출되는 함수에서는 그 주소 값을 통해 접근하여 값을 수정하는 것이 가능합니다.
</details></br>

<details>
    <summary style="font-size : 20px;"><strong> Q. 인터페이스와 추상 클래스의 차이는 무엇인가요?  </strong></summary></br>
      
추상 클래스는 필요한 대부분의 기능을 구현하고 자식 클래스에서 재정의해야하는 부분을 추상 메서드로 선언하여 기능을 확장하는데 목적이 있습니다. 추상 클래스는 abstract 키워드로 선언되며 추상 메서드를 가질 수 있는 클래스입니다. 추상 클래스 자체는 객체로서 생성될 수 없고 하위 클래스에서 추상 메서드를 구현하므로써 생성할 수 있습니다. 논리적인 측면에서 흔히 말하는 상속 관계의 A is B처럼 추상 클래스는 하위 클래스와 계층 관계가 명확해야합니다. 논리적으로 타당하더라도 클래스의 다중 상속은 불가능하므로 이미 계층 관계가 있는 클래스에 추상 클래스를 상속받게할 수 없습니다.  

인터페이스는 구현 객체에서 같은 동작을 보장하기 위한 목적입니다. 인터페이스는 interface 키워드로 선언되며 인터페이스는 객체로서 생성될 수 없습니다. 인터페이스는 추상 메세드로 구성되지만 자바 8부터는 default메서드가 추가되서 기본 동작을 구현할 수 있습니다. 인터페이스는 하위 클래스에서 여러 개의 인터페이스를 구현하도록 할 수 있으며 인터페이스끼리는 다중상속이 가능합니다. 인터페이스는 추상 클래스와 다르게 논리적인 측면에서 좀 더 자유롭습니다. 어떤 클래스의 주된 타입을 정의하는 것 이외에도 Comparable같은 부가적인 기능을 mixin할 수 있습니다. 다중 상속이 가능하므로 이미 다른 클래스를 상속받거나 다른 인터페이스를 구현하고 있는 클래스에 대해서도 새로운 인터페이스를 구현하도록 할 수 있습니다. 

</details></br>

<details>
    <summary style="font-size : 20px;"><strong> Q.  checked exception과 uncheck exception의 차이는 무엇인가요?  </strong></summary></br>
      
checked exception은 컴파일 단계에서 확인되는 예외로서 IOException, SQLException등이 포함되고 코드상으로 예외처리를 작성해야합니다. 또한, 예외 발생시 트랜잭션이 rollback되지않습니다.   

unchecked exception은 런타임시 확인되는 예외로서 RuntimeException을 상속 받습니다. NullPointerException, IllegalArgumentException등이 포함됩니다. 코드상에서 명시적으로 예외를 처리하지 않아도 실행가능하고 예외 발생시 트랜잭션시 rollback됩니다.

</details></br>

<details>
    <summary style="font-size : 20px;"><strong> Q. JAVA의 ArrayList, LinkedList차이를 설명해주세요. </strong></summary></br>
      
ArrayList는 배열 기반 리스트입니다. 자료 구조의 특성상 배열은 random access로 데이터를 접근하여 조회의 시간 복잡도는 O(1)이지만, 데이터를 삭제하고 추가하는데 원소의 위치를 시프트해서 조절하거나, 배열의 크기 이상으로 원소가 추가되는경우 배열의 크기를 조정하고 원소를 복사합니다. 그런 이유로 삭제와 추가에 대한 시간 복잡도는 O(N)입니다.  

LinkedList는 연결리스트 기반의 리스트입니다. 자료 구조의 특성상 연결되어있는 노드의 조회는 빠르지만 특정 순서의 노드를 조회하기위해서는 순차 탐색을 진행해야합니다. 따라서 조회의 시간 복잡도는 O(N)입니다. 삽입과 삭제의 경우 해당 노드의 포인터 값만 변경해주면 되기 때문에 시간 복잡도는 O(1)입니다.  

Vector는 ArrayList와 유사합니다. 차이점은 Vector는 구현 코드를 확인하면 synchronized가 메서드에 적용되어 thread safe하다는 특징이있습니다.   

조회가 빈번하다면 ArrayList가 효율적이고, 데이터의 삽입, 삭제 작업이 빈번하면 LinkedList가 성능상 좋습니다.

</details></br>

<details>
    <summary style="font-size : 20px;"><strong>  Q. Java의 HashSet, LinkedHashSet, TreeSet을 비교해주세요. </strong></summary></br>
      
HashSet은 객체를 저장하기 전에 hashcode()메서드를 호출하여 얻어낸 hash값으로 기존에 저장된 객체의 hashcode와 비교합니다. 만약 동일한 hashcode를 가진 객체가 있다면 equals()로 비교합니다. 두 객체가 다르다면 LinkedList형태로 데이터를 추가합니다. HashSet은 삽입 순서를 유지하지 않고 최대 한 개의 Null을 허용합니다. 삽입/삭제/contains의 복잡도는 O(1)입니다.  

LinkedHashSet은 HashSet과 다르게 데이터의 저장 순서를 유지합니다. 순서 유지를 위해 포인터 값을 저장하므로 HashSet에 비해 약간 느린 성능을 보입니다. 삽입/삭제/contains의 복잡도는 O(1)입니다.    

TreeSet은 특정 조건에 맞춰 데이터를 정렬하여 저장합니다. 내부적으로 Red-Black Tree를 사용합니다. 삽입과 삭제 과정에서 재정렬이 이뤄지기때문에 시간 복잡도는 O(logN)이며 조회시에도 트리를 탐색해야하므로 O(logN)의 시간이 걸립니다. Null값은 허용되지 않습니다.  

저장 순서가 유지되야하면 LinkedHashSet, 정렬되어야 한다면 TreeSet, 이외에는 HashSet을 사용하는 것이 성능상 좋습니다.

</details></br>

<details>
    <summary style="font-size : 20px;"><strong> Q. Java의 HashTable, HashMap, LinkedHashMap, TreeMap을 비교해주세요.  </strong></summary></br>
    
HashTable은 Key, Value형태로 데이터를 저장할 수 있는 구조로서 Thread safe이 보장됩니다. 삽입 순서가 유지되지 않고 동기화로 인해 HashMap보다는 느립니다. Key 값으로 Null을 허용하지 않습니다. 조회/삭제/삽입의 시간 복잡도는 O(1)입니다.  

HashMap은 thread safe하지 않고 삽입 순서도 보장되지 않습니다. Key값으로 최대 한 개의 Null값을 허용합니다. 조회/삭제/삽입의 시간 복잡도는 O(1)입니다

LinkedHashMap은 삽입 순서를 유지합니다. 순서 유지를 위해 포인터 값을 저장하므로 HashMap에 비해 약간 느린 성능을 보입니다. 조회/삭제/삽입의 시간 복잡도는 O(1)입니다

TreeMap은 특정 조건에 맞춰 데이터를 정렬하여 저장합니다. 내부적으로는 이진 트리를 사용하며 삽입, 삭제시 트리를 정렬해야하므로 시간 복잡도는 O(logN)이며 조회시에도 트리를 탐색해야하므로 O(logN)의 시간이 걸립니다. Null값은 허용되지 않습니다.     

Thread Safe가 필요한 경우 HashTable, 삽입 순서가 유지되야한다면 LinkedHashMap, 정렬 순서가 유지되어야한다면 TreeMap, 이외에는 HashMap을 사용하는 것이 성능상 좋습니다. Thread Safe이 필요한 상황에서라면 ConcurrentHashMap도 존재합니다. HashTable이 메서드 단위로 동기화를 건다면 ConcurrentHashMap은 블록 단위로 동기화를 걸어 성능상 더 뛰어납니다.    
</details></br>
    
<details>
    <summary style="font-size : 20px;"><strong> Q. String, StringBuffer, StringBuilder 차이는 무엇인가요?  </strong></summary></br>
 
String은 불변 객체로서 한번 생성되면 변경이 불가능합니다. 따라서 String을 변경하고 싶다면 새롭게 객체가 만들어야합니다. String 객체는 +연산으로도 새롭게 만들어지는데, 이런 연산은 매번 String 객체를 만드는 문제가 있습니다.  

StringBuilder는 append()를 사용하여 문자를 쌓았다가 toString()메서드가 호출되는 시점에서 String 객체를 생성합니다.    

StringBuffer는 StringBuilder와 방식이 동일하지만 thread safe한 특징이 있습니다.    
</br></br>
</details></br>

