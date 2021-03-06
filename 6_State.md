

6. 상태(State)
- 객체는 외부에 드러나는 행위와 행위를 지원하기 위한 상태를 묶어주는 편리한 단위다
- 객체 사용의 장점은 프로그램에 있는 모든 상태를 잘게 쪼개서 적당한 곳에 저장할 수 있다는 것이다
- 객체를 사용하면 참조할 수 있는 상태의 네임스페이스가 훨씬 작아지므로, 특정 변화에 의해 상태가 어떤 영향을 받을지 분석하기 쉽다

- 상태
    - 시간에 따라 변화하는 값을 사용하여 연산한다
    - 효과적으로 상태를 관리하기 위한 키포인트는 유사한 상태를 묶어서 관리하고 각 상태를 별도록 관리 하는 것이다
      두개의 상태가 동일한 연산안에서 사용되고, 동일한 시점에 생성되고 소멸되는지 보면 된다
      함께 사용되고 생성 소멸 시점이 동일한 두 가지 상태를 밀접하게 관리하는 것은 좋은 아이디어일 가능성이 높다

- 접근
    - 상태에 대한 접근을 제한해서 유연성을 조절한다
    - 하나의 프로그래밍 언어는 저장된 값에 대한 접근과 계산으로 나눌 수 있다
    - 따라서 자장과 계산의 경계를 명확하게 하는 것이 좋다
    - 접근의 용이성을 위해 객체 간의 독립성을 포기하는 것은 바람직하지 않다

- 직접 접근
    - 객체 내의 상태를 직접 접근한다
    - 데이터를 가져오거나 저장하는 것을 나타내는 가장 간단한 방법은 직접 변수를 접근하는 것이다
        - ex) x = 10;
    - 표현의 명확성이 장점이다 -> 유연성이 희생된다
    - 프로그래밍할 때 프로그래머가 사고하는 수준보다 낮다
        - doorOpen = 1; -> openDoor();
    - 직접 접근(생성자 내부만, 하위 클래스 까지만 이든 뭐든)을 어디까지 허용 할지는 개인에게 달려있다

- 간접 접근
    - 좀더 나은 유연성을 위해 메소드를 통해 상태에 접근한다
    - 접근자 메소드를 사용하면 명확성과 직접성을 희생해서 유연성을 얻을 수 있다
    - 클래스 내부에서는 직접 접근을 허락하지만 클래스 외부에서는 간접 접근을 사용하는 것을 추천한다
    - 간접 접근이 유용한 경우는 2개 이상의 데이터 간에 의존 관계가 존재하는 경우가 있다
```java
Rectangle void setWidht(int width){
    this.width = width;
    area = width * height; // 2개 이상의 데이터 간에 의존 관계가 있는 경우
}
// 이런 방식으로 해결 할 수 있지만 이것도 그닥 좋은 방법은 아니다
Widget void setBorder(int width){
    this.width = width;
    notifyListeners();
}
```

- 공용 상태 
    - 클래스의 모든 인스턴스에 적용되는 상태는 필드에 저장한다
    - 여러 연산에서 같은 데이터 요소를 사용하는 경우가 많다
      이런 경우는 클래스에 필드를 선언해서 사용 하는 것이 좋다
    - 공용 상태를 잘 만들어 놓으면 사용자는 어떻게 해야 객체의 기능을 제대로 사용 할수 있는지 쉽게 알 수 있다
    - 각 객체의 공용 상태는 모두 범위와 생명기간이 같아야 한다
    - 특정 메소드가 수행되는 도중에만 유효한 필드를 생성하고 싶을 때도 있다
      이런경우 적절한 장소에 데이터를 저장하면(인자를 사용하던지, 도우미 객체를 사용하던지) 설계를 개선할 수 있다
```java
// Point 클래스에서는 x,y값이 무조건 필요 하기 때문에 x,y를 필드로 사용 하는 것이좋다
class Point {
    int x;
    int y;
}
```

- 가변 상태
    - 같은 클래스의 인스턴스마다 다른 상태를 유지해야 할 경우 상태를 맵에 저장한다
    - 같은 객체에서도 인스턴스에 따라 각각 다른 데이터 요소를 필요로 할 때도 있다(값만 다른게 아니라 전혀 새로운 요소)
    - 가변 상태는 데이터 요소의 이름을 키로 하고 값을 데이터로 하는 맵으로 표현 된다
```java
class FlexibleObject {
    Map<String, Object> properties = new HashMap<String, Object>();
    Object getProperty(String key){
        return properties.get(key);
    }
    void setProperty(String key, Object value){
        properties.set(Key, value);
    }
}
```
    - 가변상태는 공용상태에 비해 훨씬 유연하지만, 코드 커뮤니케이션이 쉽지 않다
    - 각 필드의 상태에 따라 다른 필드를 필요로 하는 경우에는 가변 상태의 사용을 정당화할 수 있다
        - ex> A라는 필드가 참인 경우에만 B,C가 필요할 경우
    - 공용 상태를 사용하는경우, 객체의 모든 변수는 생명기간이 동일 해야한다는 원칙을 어기게 된다
    - 가능하다면 공용상태를 사용하고, 경우에 따라 어떤 필드가 필요할지 확실치 않은 경우에만 가변 상태를 사용하는 것이 좋다

- 외재 상태(Extrinsic state)    
    - 객체와 연동된 특수 상태는 상태의 사용자가 소유하는 맵에 저장 한다
    - 프로그램의 일부에서만 객체의 특정 상태를 필요로 하는 경우가 있다
        - 어떤 객체와 관련된 특수 목적 정보는 객체가 아니라 그 객체를 필요로하는 부분에 저장하는 편이 낫다
    - 외재 상태를 사용하면 객체의 복사가 어려워 진다 + 디버깅도 어려워 진다

- 변수
    - 변수는 상태 접근에 필요한 네임스페이스를 제공한다
    - 대부분의 경우에는 지역 변수를 사용하고 간간히 정적 변수와 전용 변수를 사용해서 객체 간의 의존성을 줄이는 것이 좋다
        - 변수의 종류를 줄이면, 문맥만으로도 현재 사용하는 변수가 지역 변수인지 필드인지 쉽게 구분 할 수 있다
    - 가급적 변수의 생명기간은 변수의 범위와 가까워지도록 해야 한다
    - 같은 범위에서 정의되는 변수들은 모두 같은 생명기간을 갖게 해야 한다

- 지역 변수
    - 지역 변수는 단일 범위 내에서만 유효한 상태를 저장한다
    - 가급적 정보를 널리 퍼뜨리지 않는 것이 좋다는 원칙에 따라, 지역 변수는 사용되기 직전에 가급적 최소 범위 내에서 선언해라
    - 지역 변수의 역할
        - 컬렉터   
            - 이후 사용을 위한 정보를 모은다. 때로 컬럭터의 값은 함수를 통해 반환되기도 한다
              이런 경우, 이름을 result또는 results로 짓는 것이 좋다
        - 카운터
            - 특정 객체의 수를 저장하는 특수 컬렉터
        - 설명
            - 복잡한 표현을 해야 하는 경우, 표현 내용을 지역 변수에 저장하면 독자가 좀더 쉽게 이해 할 수 있다
            - 연산에 반드시 필요한 경우가 아니더라도 지역 변수를 사용하면 복잡한 표현을 쉽게 나타낼 수 있따
            - 설명을 목적으로 하는 지역 변수는 도우미 메소드로 바꿀 수도 있다
        - 재사용
            - 값이 바뀌지만 가존 값을 다시 사용해야 하는 경우, 그 값을 지역 변수에 저장 할 수 있다
            ```java
            for(Clock each: getClocks())
                each.setTIme(System.currentTImeMillis());

            long now = System.currentTimeMillis(); // 변수에 값을 저장한다
            for(Clock each: getClocks())
                eash.setTime(now);
            ```
        - 원소
            - 지역 변수는 현재 사용하는 컬렉션의 원소를 저장하기 위해 사용 될 때도 많다 (위 에서 each)
            - 중첩 루프의 경우, 컬렉션 이름을 지역 변수 이름 뒤에 붙여서 구별하면 된다
            ```java
            broadcast() { // 중첩 루프가 되면 each 대신에 each뒤에 뭘 더 써준다
                for(Source eashSender : getSenders()) 
                    for(Destination eachReceiver : getReceivers())
            }
            ```

- 필드
    - 필드는 객체가 생성될 때부터 소멸 될 때까지 상태를 저장한다
    - 필드의 범위와 생명기간은 필드를 갖고 있는 객체와 같다
    - 필드는 객체 전번에 걸쳐 사용되므로 모든 필드는 클래스의 가장 앞이나 뒤에 한꺼번에 선언하는 것이 좋다
        - 앞에쓰면 -> 독자에게 코드에서 어떤 필드들이 사용될지 문맥을 제공해줄 수 있다
        - 뒤에쓰면 -> 행위가 가장 중요하고 데이터는 구현 세부 사항이니 신경 쓸 필요 없다고 말하는 것
    - 필드의 역할
        - 도우미
            - 도우미 필드는 객체의 여러 메소드에서 사용하는 객체를 저장한다
            - 여러 메소드에서 객체를 파라미터로전달 받는다면, 파라미터를 도우미 필드로 바꾸고 생성자에서 필드를 설정해라
        - 플래그
            - 플래그에 대한 설정 메소드가 있다면 이객체는 생명기간 동안 동작 방식이 변함을 의미한다
        - 전략
            - 객체의 연산을 하는 다른 방법이 있음을 나타내는 경우, 그 부분을 수행하는 객체를 필드에 저장하라
            - 객체의 생명기간 동안 연산 방법이 바뀌지 않는 경우라면, 생성자에서 전략 필드를 설정하라
            - 그렇지 않다면 전략 필드를 수정하는 메소드를 제공하라
        - 상태
            - 객체의 행위 양식을 결정한다는 점에서 상태 필드는 전략 필드와 비슷하다
            - 하지만 상태 필드는 스스로 다음 상태를 설정하고, 전략필드는 다른 객체에 의해 설정된다
        - 부속
            - 해당 객체가 소유하는 객체나 데이터를 저장한다

- 파라미터
    - 파라미터는 메소드가 활성화된 동안 상태를 전달한다
    - 비전용 변수(필드 혹은 정적 필드)를 사용하지 않고 상태를 다른 객체에 전달하려면 파라미터를 사용해야 한다
    - 정적 변수와 파라미터를 모두 사용할 수 있는 경우라면 언제나 파라미터를 사용하는 편이 낫다
    - 파라미터 사용으로 인해 발생하는 의존성은 영구적인 참조를 통해 발생하는 의존성보다 약하다
    - 하나의 객체에서 다른 객체에 대한 여러 메시지가 같은 파라미터를 필요로 한다면,
      그 파라미터를 호출되는 객체에 포함 시키는 것이 나을 수 도 있다
    ```java
    Server s = new Server(); // 이런 식으로 하게 된다면
    s.a(this);
    s.b(this);
    s.c(this);
    s.d(this);

    Server s = new Server(this); // 이렇게 바꿔라
    s.a();
    s.b();
    s.c();
    s.d();
    ```

- 수집 파라미터
    - 여러 개의 메소드를 통해 복잡한 결과를 얻기 위해 파라미터를 전달한다
    - 여러 메소드 호출을 통한 결과를 모으려면 결과를 통합하는 과정이 필요한다
    - 한가지 방법은 메소드의 결과값을 반화하는 것이다. 이 방법은 값이 정수처럼 단순한 경우 적합하다
    ```java
    int size(){
        int result = 1;
        for(Node each : getCHilderen())
            return += each.size();
        return result;
    }
    ```
    - 하지만 단순 덧셈이 아니라 좀더 복잡한 방식을 통해 결과를 통합해야 하는 경우라면,
      파라미터를 전달해서 결과를 수집하는 편이 더 직관 적이다
    ```java
    asList(){
        List results = new ArrayList();
        addTo(results);
        return results;
    }
    addTo(List elements){
        elements.add(getValue());
        for(Node each : getChildren())
            each.addTo(elements);
    }
    ```

- 파라미터 객체
    - 자주 사용하는 긴 파라미터 목록은 객체로 만들어서 통합한다
    - 반드시 필요한 파라미터를 앞에서 전달하고, 옵션 파라미터는 뒤에 전달한다
      이렇게 하면 파라미터를 가급적 바꾸지 않고 옵션 파라미터를 선택적으로 사용 할 수 있다

- 가변 인자
    - 어떤 메소드는 특정 타입의 파라미터를 여러 개 취할 수도 있다 -> 지저분 해 진다
    ```java
    Collection<String> keys = new ArrayList<String>();
    keys.add(key1);
    keys.add(key2);
    object.index(keys);

    // 메소드를 methos(Class... classes)와 같이 선언하면, 다음과 같이 임의수 수의 인자를 사용하여 메소드를 호출할 수 있다
    object.index(key1, key2);
    ```

- 파라미터 객체
    - 여러 개의 파라미터가 함께 여러 메소드로 전달된다면 이들을 묶어서 하나의 객체로 만드는 것을 고려할 수 있다
    - 파라미터 리스트를 하나의 객체로 바꾼후, 일부 코드에서 파라미터 객체의 일부 필드만 사용한다면 그에 해당하는
      메소드를 파라미터 객체에 만들면 된다
      ```java
      setOuterBounds(x, y, width, height);
      setOuterBOunds(x + 2, y + 2, width - 4, height - 4);

      setOuterBounds(bounds);
      setOuterBounds(bounds.expand(-2));
      ```
    - 객체를 파라미터로 만들어 사용하면 객체를 만들어야 하기때문에 성능 저하가 있지만 무시해도됨

- 상수
    - 변하지 않는 상태는 상수로 저장한다
    - 변화하지 않는 데이터를 프로그램의 여러부분에서 사용해야 하는 경우, 컴파일 할때 그값을 알고 있다면
      변수를 static final로 선언하고 프로그램에서 그 변수를 참조하게 하라
    - 흔히 상수 변수는 다른 변수들과 구별하기 위해 변수 이름에 대문자만 사용하는 경우가 많다
    - 상수를 사용하는 가장 큰 이유는 상수 이름ㅇㄹ 통해 그 값의 뜻을 전달할 수 있기 때문이다

- 역할 제시형 작명
    - 변수 이름은 연산에서의 역할을 반영하여 짓는다
    - 그냥 알아서 잘 지어라 나도 모르겠다

- 선언형 타입
    - 변수에 대한 일반적 타입을 선언한다
    - 타입 이름에는 구현을 반영하기보다는 역할을 반영해야 한다
    - 그냥 알아서 잘 지어라 나도 모르겠다

- 초기화
    - 변수 초기화는 가급적 선언적으로 한다
    - 초기화는 변수가 사용되기 전에 알고 있는 상태로 만드는 작업이다
    - 초기화와 선언은 가급전 한곳에서 하는 것이 좋다
    - 초기화에 드는 비용이 비싼 변수의 경우, 생성과 초기화를 분리하는 것이 나을 수 있따

- 열성적 초기화
    - 인스턴스가 생성될 때 필드를 초기화한다
    - 열성적 초기화는 변수가 선언되거나 생성되자마자 초기화한다

- 게이른 초기화
    - 초기화 비용이 높은 객체의 경우, 객체가 실제 사용되기 직전에 초기화 한다
