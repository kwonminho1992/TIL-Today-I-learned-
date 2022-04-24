###Compile vs Interpret

- Compile : 개발자가 작성한 코드를 컴퓨터가 이해할 수 있는 언어로 바꾸는 작업

- Interpret : 개발자가 작성한 코드를 컴퓨터가 이해하면서 실행 같이도 함

###Java의 특징

1. 플랫폼(app이 실행되는 환경)의 독립성 - 컴파일 후 인터프리트를 해야하는 언어
    - java platform의 종류
        - Java SE(Standard edition) - UI / Network 可能
        - Java EE(Enterprise edition) - Servlet / JSP / EJB 可能 (현재 EJB는 잘 안씀)
        - Java ME(Micro edition) - Mobile  可能 (현재 Mobile은 잘 안씀)
2. Memory 관리를 알아서 한다 (JVM을 통해 관리)
3. 실행 時에 메모리를 확보함 
4. 자바 플랫폼의 구성
    - JVM (Java virtual machine)
        - 자바는 컴파일 時, byte 단위로 컴파일 됨. byte를 bit 단위로 바꾸어 실행하기 위해 필요한 프로그램이 JVM (*byte 단위로 컴파일을 하면 다른 운영체제에서도 원활히 실행됨)
    - API (Application programming interface)
        - 개발자들의 개발을 쉽게 해주는 library
    - JRE (Java runtime environment) = JVM + API
    - JDK (Standard edition development kit) = JRE + compiler + interpreter + 문서화응용프로그램 v.v....
        - JDK 8 : JRE의 버전이 높을 수록 사용할 수 있는 라이브러리의 개수가 적어져서 가장 안전한 8버전을 많이 사용 (그 다음으로는 11버전을 많이 씀)

###명령프롬프트에서 자바 실행하기

1.  환경변수 설정 : 어느 디렉토리에서나 컴파일&실행을 가능하게 설정하는 것 (설정법은 구글 검색으로 찾아볼 것)
2. 컴파일
    1. option 1 : 같은 디렉토리 안에 컴파일하기
        **$ javac src디렉토리\자바파일.java**
    2. option 2 : bin 디렉토리 안에 컴파일하기
        **$ javac -d bin디렉토리 src디렉토리\자바파일.java**
3. 파일 실행
    **$ java -cp 디렉토리 클래스이름 (ex. java -cp D:\244\mySE\bin First)**
4. 문서화
    1. html 페이지를 생성함
    2. class, method, constructor 등의 바로 위에 document comment를 달아 해당 코드에 대해 문서화된 사용법을 만들 수 있다
    **$ javadoc -d doc src디렉토리\자바파일.java**

![Java_run_procedure](./01.jpg)

###자료형 (Data type)

1. 기본형(primitive) : 값을 직접 가지고 있는 유형
    1. 숫자형
        1. 정수
            1. 종류 : byte(메모리 1byte 사용), short(메모리 2byte 사용), int(메모리 4byte 사용), long(메모리 8byte 사용)
        2. 실수 
            1. 종류 : float(메모리 4byte 사용), double(메모리 8byte 사용)
            2. 특징 : 지수를 활용하기 때문에 정수보다 더 긴 숫자를 표현 가능
    2. 문자형
        1. 종류 : char(메모리 2byte 사용)
        2. 문자조합
            1. ASCII (7 bit, 128가지)
            2. Multi byte (영어 外 문자 표기용) - MS949, CP949 等
            3. Unicode (전세계 모든 문자 표기, 표준)
                1. 2byte 기반 : 기본 (Java의 기본 문자조합)
                2. 1~3byte 기반 : 영어는 1byte만 사용, 다른 언어는 2~3byte를 사용 (효율적인 메모리의 사용을 위함 → UTF-8)
    3. 논리형
        1. 종류 : boolean (메모리 1byte 사용)
        2. 특징 : 다른 자료형과 형변환이 불가능 (true or false 값만 가지도록 고정되어있음)
2. 참조형(reference) : 메모리에 있는 값의 주소를 가리키(point)는 유형 
    1. 종류
        1. Array (각 index 당 메모리 4byte 사용) 
            1. 정수형은 기본값이 0, 실수형은 기본값이 0.0, 논리형은 기본값이 false, 문자형과 참조형은 기본값이 null로 초기화
            2. array에는 index 묶음의 메모리의 정보값이 들어있음
        2. String (文字列)
            1. 복수의 문자를 하나의 변수 안에 넣을 수 있음
            2. 선언방법
                1. String str = new String(”Hello World”);
                2. String str = “Hello World”;
        3. Map
            1. key : value 의 구조
            2. 크기가 가변적임 (추가, 삭제가 용이함)

**연산자 (Operator)**

1. 이항(二項)연산자 (binary operator)
    1. 산술연산자 (arithmetic operator)
        1. 종류 : +, -, *, /, %
    2. 대입연산자 (assignment operator)
        1. 종류 : =, +=, -=
    3. 비교연산자 (comparison operator)
        1. 종류 : >, <, >=, <=, ==, !=
        2. 특징 : 결과값이 항상 boolean type으로 반환됨
    4. 논리연산자 (logical operator)
        1. 종류 : and(&&), or(||), not(!)
        2. &&(||). &(|)의 차이 : 앞의 조건이 false인 경우 &&(||)는 뒤의 조건과 상관없이 더이상 연산을 안하고 결과를 반환함. 반면에 &(|)는 계속 연산을 한 뒤 결과를 반환함 
2. 단항(單項)연산자 (unary operator)
    1. 종류 : ++. —
    2. 주의 : 다른 연산자와 같이 사용하지 않는 것을 권장
3. 삼항(三項)연산자 (ternary operator)
    1. 형태 : 조건식 ? 값1 : 값2; 
    2. 특징 : 조건식이 true이면 값1 출력, 조건식이 false이면 값2 출력

```java
// 삼항(三項)연산자 (ternary operator) 예시 코드
int a = 12;
a % 2 == 0 ? "even" : "odd";

//result
even
```

**조건문 (Condition)**

1. 종류
    1. if condition
    2. switch condition

**반복문 (Loop)**

1. 종류
    1. for loop
    2. while loop
    3. do - while loop
    
    ```java
    //do ~ while loop 
    //loop the code until n = 1	
    int n;
    do {
    	n = (int) (Math.random() * 5);
    	System.out.println(n);
    } while ( ! (n == 1) );
    ```
    
2. 반복문의 제어
    1. break : (보통 if문과 같이 쓰임) 조건에 걸리면 해당 반복문을 끝내고 빠져나옴
    2. continue : (보통 if문과 같이 쓰임) 조건에 걸리면 연산을 하지 않고 건너 뜀. (반복문을 끝내지는 않음)

