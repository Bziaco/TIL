# 클래스와 멈버의 접근 권한을 최소화하라

## 1. 정보은닉 혹은 캡슐화

### 1.1 잘 설계된 컴포넌트란?

- 클래스 내부 데이터와 내부 구현 정보를 외부 컴포넌트로부터 **얼마나 잘 숨겼느냐의 차이**이다.

- 내부 구현을 완벽히 숨겨 구현과 API를 깔끔히 분리한다. 오직 API를 통해서만 다른 컴포넌트와 소통하며 서로의 내부 동작 방식에는 전혀 개의치 않는다.

- 이를 정보 은닉 혹은 캡슐화라고 한다.

### 1.2 정보은닉의 장점 5가지

#### 1.2.1 시스템 개발 속도를 높인다. 여러 컴포넌트를 병렬로 개발할 수 있기 때문이다.

#### 1.2.2 시스템 관리 비용을 낮춘다.

- 각 컴포넌트를 더 빨리 파악하여 디버깅할 수 있고 다른 컴포넌트로 교체하는 부담도 적기 때문이다.

#### 1.2.3 정보 은닉 자체가 성능을 높혀주진 않지만 성능 최적화에 도움을 준다.

- 완성된 시스템을 프로파일링해 최적화할 컴포넌트를 정한 다음 다른 컴포넌트에 영향을 주지 않고 해당 컴포넌트만 최적화 할 수 있다.

#### 1.2.4 소프트웨어 재사용성을 높인다.

- 외부에 거의 의존하지 않기 때문에 낮선 환경에서도 유용하게 쓰일 가능성이 크다.

#### 1.2.5 큰 시스템을 제작하는 난이도를 낮출 수 있다.

- 컴포넌트 별로 단위 테스트를 통해 컴포넌트 동작을 검증할 수 있기 때문이다.


<br>

## 2. 모든 클래스와 멈버의 접근성을 가능한 좁혀야 한다.

### 2.1 자바는 정보 은닉을 위한 다양한 장치를 제공한다.

- 접근제어 메커니즘은 클래스, 인터페이스, 멤버의 접근성을 명시한다.

- 각 요소의 접근성은 그 요소가 선언된 위치와 **접근 제한자(private, protected, public)**로 정해진다.

- **이 접근 제한자를 제대로 활용하는 것이 정보 은닉의 핵심이다.**

### 2.2 톱테벨(가장 바깥이라는 의미) 클래스와 인터페이스에 부여할 수 있는 접근 수준은 package-private과 public이다.

- **톱레벨 클래스나 인터페이스를 public으로 선언하면 공개 API**가 되며 **package-private으로 선언하면 해당 패키지 안에서만 이용**할 수 있다.

- **패키지 외부에서 쓸일이 없다면 package-private로 선언하자**. 그러면 클라이언트에 아무런 피해 없이 다음 릴리스에서 수정, 교체, 제거 할 수 있다.

- 반면 public으로 선언한다면 API가 되므로 하위 호환을 위해 영원히 관리 해줘야 한다.

### 2.3 package-private 클래스/인터페이스 내에 private static 중첩

- 톱레벨에 package-private를 두면 패키지 내 모든 클래스가 접근 할 수 있지만 private static을 중첩시키면 바깥 클래스 하나에서만 접근 할 수 있다.

### 2.4 public일 필요가 없는 클래스의 접근 수준을 package-private 톱레벨 클래스 해라.

### 2.5 멤버에 부여할 수 있는 접근 수준은 네 가지다.

- 멤버란 필드, 메서드, 중첩 클래스, 중첩 인터페이스를 말한다.

- 접근 범위가 좋은 것부터 순서대로 살펴보자.

    - private: 멤버를 선언한 톱레벨 클래스에서만 접근할 수 있다.

    - package-private: 멤버가 소속된 패키지 안의 모든 클래스에서 접근할 수 있다. 접근 제한자를 명시하지 않았을 때 적용되는 패키지 접근 수준이다.(단, 인터페이스의 멤버는 기본적으로 public이 적용된다.)

    - protected: package-private의 접근 범위를 포함하며 이 멤버를 선언한 클래스의 하위 클래스에서도 접근할 수 있다.

    - public: 모든 곳에서 접근 할 수 있다.

### 2.6 API 내 접근 수준 설정 방법

- 공개 API 설계 후 **그 외 모든 멤버는 private**로 만든다.

- 그런 다음 오직 **같은 패키지의 다른 클래스가 접근해야 하는 멤버에 한하여 package-private**으로 풀어주자

- 권한을 풀어주는 일을 자주하게 된다면 시스템에서 컴포넌트를 더 분해해야 하는 것은 아닌지 고민해야 한다.


#### 2.6.1 public 클래스에서서 멤버 접근 수준을 protected로 바꾸면 접근 대상 범위가 엄청나게 넓어진다.

- public 클래스의 protected 멤버는 공개 API이므로 영원히 지원되어야 한다.

- 또한 내부 동작 방식을 API 문서에 적어 사용자에게 공개해야 할 수도 있다.

- **따라서 protected 멤버의 수는 적을수록 좋다.**

#### 2.6.2 멤버 접근성을 좁히지 못하게 하는 방해하는 제약

- **상위 클래스의 메서드를 재정의할 때는 그 접근 수준을 상위 클래스에서 보다 좁게 설정 할 수 없다.**

- 상위에서 중요하게 생각하는 멤버라면 하위에서도 중요하다라는 *리스코프 치환 원칙*을 지키기 위해서 이다.


#### 2.6.3 코드 테스트를 위한 접근 제한자는 package-private까지만 허용하라.

#### 2.6.4 public 클래스의 인스턴스 필드는 되도록 public이 아니어야 한다.

- 필드가 가변 객체를 참조하거나 final이 아닌 인스턴스 필드를 public으로 선언하면 그 필드에 담을 수 있는 값을 제한할 힘을 잃게 된다. 그 필드와 관련된 모든 것은 불변식을 보장할 수 없게 된다는 뜻이다.

- public 가변 필드를 갖는 클래스는 일반적으로 스레드에 안전하지 않다.

#### 2.6.5 추상 개념을 완성하는데 필요한 구성요소로써 상수라면 public static final 필드로 공개해도 좋다.

- **관럐상 상수 이름은 대문자 알파벳으로 쓰며 각 단어 사이에 밑줄(_)을 넣는다.**

- 이런 필드는 반드시 **기본 타입 값이나 불변객체를 참조**해야 한다.

#### 2.6.6 클래스에서 public static final 배열 필드를 두어선 안된다.

- 길이가 0이 아닌 배열은 모두 변경 가능하니 주의해야 한다.

- 클래스에서 **public static final 배열 필드를 두거나 이 필드를 반환하는 접근자 메서드를 제공해서는 안된다.**

- 아래 코드는 보안 헛점이 존재한다. public으로 배열이 선언되었기 때문에 **언제든 수정이 가능하게 된다는 헛점**이 있다.

    ```js
    public static final Thing[] VALUES = {....};
    ```

- 해결 방법은 2가지가 있다. 

    - public 배열을 private으로 만들고 public 불변 리스트를 추가하는 것이다.

        ```js
        private static final Thing[] PRIVATE_VALUES = {...};
        public static final List<Thing> VALUES = 
            Collections.unmodifiableList(Arrays.asList(PRIVATE_VALUES));
        ```
    
    - 배열을 private로 만들고 그 복사본을 반환하는 public메서드를 추가하는 방법(방어적 복사)

        ```js
        private static final Thing[] PRIVATE_VALUES = {...};
        public static final Thing[] values() {
            return PRIVATE_VALUES.clone();
        } 
        ```

<br>

## 3. 모듈 시스템

### 3.1 모듈이란?

- 자바 9에서는 모듈 시스템이라는 개념이 도입되면서 *두 가지 암묵적 접근 수준*이 추가 되었다.

- **패키지가 클래스들의 묶음이듯 모듈은 패키지들의 묶음이다.**

- 모듈은 자신이 속하는 패키지 중 공개(export)할 것들을 (관례상 module-info.java 파일에) 선언한다.

### 3.2 모듈 시스템의 장점

- protected 혹은 public 멤버라도 해당 패키지를 공개하지 않았다면 모듈 외부에서는 접근할 수 없다.

- 모듈 안에서는 exports로 선언했는지 여부에 아무런 영향도 받지 않는다.

- 모듈 시스템을 활용하면 클래스를 외부에 공개하지 않으면서도 같은 모듈을 이루는 패키지 사이에서는 자유롭게 공유할 수 있다.

### 3.3 두 가지 암묵적 접근 수준

- 암묵적 접근 수준들은 각각 public 수준과 protected 수준과 같으나 그 효과가 모듈 내부로 한정되는 변종인 것이다.

- 이런 형태로 공유해야 하는 상황은 흔지 않고 그래야 하는 상황이라면 패키지들 사이에서 클래스들을 재배치하면 대부분 해결된다.

### 3.4 모듈에 적용되는 두 접근 수준은 상당히 주의해서 사용해야 한다.

- JAR 파일을 자신의 모듈 경로가 아닌 애플리케이션의 클래스패스에 두면 그 모듈안의 모든 패키지는 마치 모듈이 없는 것처럼 행동한다.

- 즉 public 클래스가 선언한 모든 public 혹은 protected 멤버를 모듈 밖에서도 접근 할 수 있게 된다.

- 이 접근 수준을 적극 활용한 예가 JDK 이다.

### 3.5 모둘 패키지 사용 방법

- 패키지들을 모듈 단위로 묶고 모듈 선언에 패키지들의 모든 의존성을 명시한다.

- 그런 다음 소스 트리를 재배치하고 모듈 안으로 부터 일반 패키지로의 모든 접근에 특별한 조치를 취해야 한다.


<br>

## 4. Conclusion

- 프로그램 요소의 접근성은 가능한 한 최소한으로 하라

- 꼭 필요한 것만 골라 최소한의 public API를 설계하라. 그 외 클래스, 인터페이스, 멤버가 의도치 않게 API로 공개되는 일이 없도록 해야 한다.

- public 클래스는 상수용 public static final 필드 외에는 어떤한 public 필드도 가져서는 안된다. public static final 필드가 참조하는 객체가 불변인지 확인하자.

