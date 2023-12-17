# 4week issue에 대한 토론 일지

---
## 회의 시나리오
## 이슈

1. 안건을 올린 이유 발표
  
2. 안건에 대해 답변자 발표 및 설명 요약
  
3. Q n A 및 회의록 기록

---
## 이슈

1. 안건을 올린 이유 발표
- Java의 null 처리 방법에는 Optional 이외에도 requiredNonNull, assert 등이 있는 것으로 알고 있습니다.
이들과 Optional은 각각 어떻게 사용되는지 차이점이나 특징, 사용사례 등에 대해 알고싶습니다.
  
2. 안건에 대해 답변자 발표 및 설명 요약
- 자바에서 null은 아주 골칫덩어리 : 자바 애플리케이션에서 가장 많이 발생하는 예외가 NPE 발생.
- null을 피하려면?
  - 매번 분기 ? : 잊을 수도 있고, 너무 코드가 복잡해짐 => null handling 필요함.
- null handling 방법 :
  - 선언과 동시에 초기화.
  - Objects.isNull(), Objects.nonNull() 사용 : if(var == null) 사용 지양 권장.
  - Objects.requireNonNull() : 가독성을 위함.
  - 타입으로 관리하기 : 레퍼런스 타입만 null을 가지기 때문에 되도록 primitive 타입 사용 권장.
  - 메서드 오버로딩 : 오버로딩을 활용하여 필요한 인자만 들어가는 메서드를 정의할 것.
  - Collections.emptyList() : Collection을 반환하는 메서드에서는 Collections.emptyList() 반환.
  - Optional :
    - null을 요소로 넣지 말 것 -> empty 활용.
    - ofNullable()을 사용하여 null 처리할 것.
- OPtional 사용시 주의사항 :
  - 멤버 변수에 Optional 선언은 안티 패턴 : 직렬화 불가 -> 메서드 반환 목적으로만 사용할 것.
  - collection에는 optional을 사용하지 말것 : Collections.empty..() 활용할 것.
  
5. Q n A 및 회의록 기록


---
## 이슈

1. 안건을 올린 이유 발표
  책에서 모듈성에서 리플렉션을 언급하면서 모듈을 읽어오는 것을 얘기했지만 강력한 기능임에도 언급만 하고 넘어가 이번 기회에 리플렉션에 대해서 알아보면 좋을 것이라고 생각함
2. 안건에 대해 답변자 발표 및 설명 요약
리플렉션?

- 객체의 클래스를 몰라도 클래스의 메서드, 멤버변수에 접근할 수 있게해주는 자바 API

리플렉션으로 가져올 수 있는 정보

- 동적으로 런타임에 클래스를 추출할 수 있어서 컴파일 상황에서 에러가 발생하지 않음
- 인텔리제이에서 자동완성, 스프링 어노테이션에서 리플렉션이 사용됨
- 클래스, 생성자, 메서드를 가져오는 기능을 제공하여 클래스의 정보를 모두 가져올 수 있음

리플렉션을 사용하면 접근제어자 상관없이 모든 필드에 접근할 수 있다
- 모듈에서는 우리의 클래스에 대한 리플렉션을 사용할 수 있는지 여부를 명시적으로 표현할 수 있음

자바에서의 사용
- 디버깅, JUnit과 같은 테스트 프레임워크에서 런타임에 알 수 없는 객체의 동작 과정 분석시
- third party 라이브러리를 사용하는 경우 해당 라이브러리에 존재하는 클래스 및 메서드를 분석 하는 경우

  
3. Q n A 및 회의록 기록


---
## 이슈

1. 안건을 올린 이유 발표
   
```java
public String getCarInsuranceName(Optional<Person> person) {
 return person.flatMap(Person::getCar)
 .flatMap(Car::getInsurance)
 .map(Insurance::getName) // (1)
 .orElse
```

(1)에서 Optional<Insurance>가 사용되어서 Optional<Optional<String>>가 반환될 줄 알았는데 Optional<String>이 반환된 이유를 알고싶습니다. Optional<Optional<>>로 반환되는지 Optional<>로 반환되는지 판단 기준을 알고 싶습니다.

2. 안건에 대해 답변자 발표 및 설명 요약

```java
public<U> Optional<U> map(Function<? super T, ? extends U> mapper) { }
public<U> Optional<U> flatMap(Function<? super T, Optional<U>> mapper) { }
```

- map
  - 반환값은 U를 상속하는 클래스에 Optional을 감싸서 반환
  - mapper의 function
- flatMap
  - mapper의 반환 타입 그대로 반환
  
4. Q n A 및 회의록 기록


---


---

## 이슈 추상 클래스와 인터페이스, Optional orElse 차이

1. 안건을 올린 이유 발표
  - 자바 8을 기준으로 차이를 생각해보면 좋겠습니다.
  - optional의 종단 메서드를 쓰는 기준을 생각해보면 좋겠습니다.

2. 안건에 대해 답변자 발표 및 설명 요약
  - 자바 8 이후에는 구현부가 있는 정적 메서드랑 디폴트 메서드가 있기 때문에 추상 클래스 처럼 구현부를 가진 메서드를 자식에게 줄 수 있게됐다.
  - 차이점은 추상 클래스는 멤버 인스턴스 변수를 사용할 수 있고, 단일 상속만 지원한다.
  - 인터페이스는 다중 상속을 지원합니다.
  - optional 내부 값이 null이 아닐 때 orElse는 함수를 받을 때 실행하고 orElseGet은 인자로 받은 함수를 실행하지 않습니다. 
  
3. Q n A 및 회의록 기록

---
## 이슈 빌드 툴

1. 안건을 올린 이유 발표
  - 빌드 툴에 대한 언급이 자주 나오는데 무엇인지 궁금합니다.
  
2. 안건에 대해 답변자 발표 및 설명 요약
  - 빌드 툴 : 소스코드가 빌드가 되도록하는 자동화된 도구
  - 빌드 : JVM이나 WAS가 인식하도록 패키징하는 과정
  - 빌드과정
    - 전처리, 컴파일, 어셈블, 링크
  - 빌드툴 역할
    - 소스코드 컴파일, 패키징
    - 테스트 자동 수행
    - 의존성 주입 및 배포
  - 필요성
    - 필요한 라이브러리들이 많아짐에 따라 복잡성이 증가함
    - 다양한 문제를 해결하기 위해 사용
  - 빌드툴 종류
    - Apache Ant
      - 가장 오래된 빌드툴
      - 규칙이나 구조를 강요x -> 유연성 장점
      - 빌드 과정의 이해 어렵고 유지보수가 어렵다.
    - Maven
      - XML 기반 스크립트 작성
      - 필요한 라이브러리 불러오는 기능 처음 추가
      - 구조화, 공식적 제약o -> 빌드 시간 단축
      - 빌드 스크립트 이해 어려움, 유연성 x
    - Gradle
      - Groovy나 Kotlin 같은 언어 기반 스크립트 작성
      - 유연성이 장점 (동적인 스크립트로 작성)
      - pom.xml 보다 더욱 간략하게 작성 가능
      - 설정 정보를 필요한 모듈에만 주입하는 Configuration Injection 가능
      - 빌드속도 최고 (Maven 2배 이상)
        - 점진적 빌드가 이유
          - 변화점이 있는지 확인 후 변한 것만 빌드
        - 빌드 캐시
        - 그레이들 데몬
  
3. Q n A 및 회의록 기록

---

## 이슈 인터페이스에서 정적 메서드와 디폴트 메서드

1. 안건을 올린 이유 발표
- 인터페이스에서 추가된 정적 메서드와 디폴트 메서드는 둘 다 구현부를 가지는데 둘을 나눈 이유와 차이점이 궁금합니다.
  
2. 안건에 대해 답변자 발표 및 설명 요약
- 유틸리티 클래스를 나눠서 사용했는데 자바 8에서 정적 메서드와 디폴트 메서드를 추가함으로서 유틸리티 클래스를 사용하지 않아도 되었지만, 하위 호환성을 위해 남아있음
  - Ex) 컬렉션도 인터페이스와 유틸 클래스가 하위 호환성 때문에 같이 존재함
- 인터페이스에 정적 메서드를 추가할 수 있는 이유
  - 확장에 용이하게 하기 위함
  - 인스턴스를 생성하지 않고 호출 가능
- 객체 지향에서 둘의 차이는 정적 메서드의 경우 객체의 상태와 무관하게 기능을 제공하고 다형성 측면에서도 제한적이라 결합도가 높지만, 디폴트 메서드는 이러한 부분에서 객체 지향을 깨지 않고 사용 할 수 있음
- 메모리적인 측면에서는 두 메서드 성능적인 측면에서는 큰 차이는 없음, 메서드 호출 시에는 동적 바인딩을 사용하는 디폴트 메서드가 느리지만, 무시 할만한 수준
- 결론적으로는 객체 지향의 패러다임을 깰 수 있는 정적 메서드 사용을 지양하고, 유틸리티 메서드를 사용해야한다.

  
3. Q n A 및 회의록 기록
- 디폴트 메서드가 호출될 때마다 인스턴스가 생성될 때마다 힙 영역에 생성이 되는가?
  - 생성이 된다. 하지만 성능 차이는 미미하다.

---

## 이슈

1. 안건을 올린 이유 발표
Java에 모듈 기능이 지원됨에 따라서 빌드 과정이 어떻게 되는지 궁금해졌습니다.
  
2. 안건에 대해 답변자 발표 및 설명 요약
컴파일 과정에서 가장 중요한 것은 JVM이다. Java는 JVM을 통해서 플랫폼에 영향을 받지 않으며 동작할 수 있게 된다.
JVM은 플랫폼에 종속적이다!

클래스 로더
- 런타임 중 JVM의 메서드 영역에 동적으로 java 클래스를 로드하는 역할
    - 동적 로딩을 통해 필요한 클래스들을 로딩 및 링크하여 JVM의 메모리에 올린다.
- 로딩 -> 링크 -> 초기화

실행 엔진
- 바이트 코드를 명령어 단위로 한 줄씩 실행한다.
- 인터프리터! 느리다.(반복문!!) -> JIT 컴파일러
- JIT 컴파일러 : 컴파일한 코드를 캐싱하고 이후에는 바뀐 부분만 컴파일한다.

4. Q n A 및 회의록 기록
- 바뀐 부분만 컴파일한다는 것? (JIT)
  반복문을 예로 들 수 있다. 같은 코드를 계속해서 반복할 떄 캐싱된 컴파일된 코드를 사용할 수 있다.

---