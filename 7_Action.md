7. 행위
- 제어 흐름
    - 연산을 여러 단계로 나태낸다
    - 프로그램을 예외를 가진 하나의 주요 흐름으로 표현할지, 동등하게 중요한 여러 흐름으로 표현할지,
      두가지 방법을 함께 사용할지 정해야 한다

- 주요 흐름
    - 주요 제어 흐름을 명확하게 표현한다
    - 대부분 프로그래머는 작성하는 프로그램의 주요 흐름이 어떤 것인지 알고 있다
      프로그래밍 언어를 통해 주요 흐름을 명확하게 표현하라
    - 흔치 않은 상황이나 에러 상황은 예외와 조건절을 사용해서 표현하면 된다

- 메시지
    - 메시지를 보내서 제어 흐름을 표현한다
    - 절차적 언어(procedural language)는 프로시저 호출을 사용해서 정보를 숨긴다
    ```java
    compute(){  //이 연산을 이해하기 위해 알아야 할 것은 3단계이며 당장 자세한 내용을 알 필요는 없다라는 뜻
        input();
        process();
        output();
    }
    ```
- 선택 메시지 (전략 패턴)
    - 여러 선택 사항을 나태내기 위해 메시지 구현자를 다양화 한다
    ```java
    public void displayShape(Shape subject, Brush brush){
        brush.display(subject)
    }
    // 어떤 brush를 사용할지는 런타임에 고를수 있도록 해준다
    ```
    - 명시적 조건문의 사용을 크게 줄일 수 있으며 추후 확장이 쉽다
    - 명시적 조건문을 사용한 부분은 나중에 전체 프로그램의 행위를 바꿀때마다 명시적으로 수정해야 한다
    - 선택 메시지를 사용하면, 독자가 연산의 세부 구현을 이해하기 위해 노력해야 한다
      따라서 당장 연산의 변형이 필요하지 않은 경우라면 미래 확장을 위해 굳이 선택 메시지를 사용할 필요는 없다

- 더블 디스패치 (방문자 패턴)
    - 두가지 축으로 메시지 구현자를 다양화해서 중첩된 선택을 표현한다
    - 두가지의 독립적인 차원에서의 변형을 표현하기 위해서는 2개의 선택 메시지를 직렬로 연결해야 한다
    - 즉, shape 도 brush를 선택 할수 있게 해주고 brush도 shape를 선택 할 수 있게 해준다
    ```java
    displayShape(Shape subject, Brush brush){
        subject.displayWith(brush);
    }

    Oval.displayWith(Brush brush){
        brush.displayOval(this);
    }
    Rectange.displayWith(Brush brush){
        brush.displayRectangle(this)
    }

    PostscriptBrush.displayRectangle(Rectangle subject){
        write print(subject.left() + " " + ... + " rect);
    }
    ```


- 분리 메시지 (템플릿 패턴)
    - 복잡한 연산은 밀접한 단위의 연산으로 나눈다
    - 여러 단게로 구성되는 복잡한 알고리즘이 있다면, 관련된 단계들을 모으고 이를 수행하기 위해 메시지를 보낼 수 있다

- 되돌림 메시지
    - 메시지를 같은 수신자에게 보내서 제어 흐름에 대칭성을 부여한다
    ```java
    void compute(){ // 대칭성이 떨어진다 -> 가독성이 좋지 않다
        input();
        helper.process(this);
        output();
    }
    // 그냥 별로 다를건 없고 보기가 좋아 졌음
    void process(Helper helper){
        helper.process(this);
    }
    void compute(){
        input();
        process(helper); // 여기가 좋아 졌음
        output();
    }
    ```

- 초청 메시지
    - 다른 방식으로 구현될 수 있는 메시지를 보내서 미래에 일어날 변형을 대비한다
    - 하위클래스의 어떤 연산을 변형시킬 것이라 예상할 수 있다. 
      이런 경우에는 적당한 이름을 메시지를 통해 변형의 여지를 마련했음을 알려주는 것이 좋다 
      -> 목적에 맞게 연산을 구현하도록 초청의 의미

- 설명 메시지
    - 로직을 설명하기 위해 메시지를 보낸다
    - 개발자의 의도와 구현을 구분하는 것은 언제나 중요하다
    ```java
    public void highlight(Rectangle area){
        reverse(area);
    }
    // reverse(area)를 바로 호출 하지 않는 것은 highlight로 개발자의 의도를 전달 하기 위함이다
    ``` 
    - 

- 예외 흐름
    - 주요 흐름에 대한 표현을 방해하지 않으면서, 가급적 명확하게 예외적 제어 흐름을 표현한다

- 보호절
    - 지역적 예외 흐름은 이른 반환을 통해 표현한다
    - 보호절을 사용하면 간단한 지역적 예외 상황을 지역적인 변화만을 수반하며 표현할 수 있다
    ```java
    void initialize(){
        if(!isInitialized()){

        }
    }
    void initialize(){
        if(isInitialized())
            return;
    }
    // 보호절은 한 쪽의 제어 흐름이 다른 쪽보다 중요한 경우 유용하다
    ```

- 예외
    - 비지역적 예외 흐름은 예외로 표현한다
    - 여러 함수 호출을 걸쳐서 제어 흐름을 바꾸는 경우를 표현할 때 유용하게 사용된다
    - 예외에는 비용이 들어 간다
    - 예외는 일종의 설계상 누수 이다
    - 가능한 순차적 구문, 메시지, 루프, 제어문을 사용해서 제어 흐름을 표현해라
    - 주요 흐름의 이해를 방해하는 경우에만 예외를 사용 사용하라

- 체크 예외
    - try catch 구문 만들때 지원해주는 catch 같은 것들(RuntimeException 말고 나머지)
    - 명시적 선언으로 예외를 처리한다
    - 체크 예외에는 비용이 따른다

- 예외 전달
    - 예외를 전달할 때에는 예외 처리자에게 적합한 정보를 전달 할 수있도록 필요에 따라 예외의 형태를 변화한다