# Primitive Type VS Wrapper Class

### 자바의 자료형

- **기본형(primitive type)**
  - `논리형(boolean)`, `문자형(char)`, `정수형(byte, short, int, long)`, `실수형(float, double)`
    계산을 위한 실제 값을 저장한다.
- **참조형(reference type)**
  - 객체의 주소를 저장한다. 기본형을 제외한 나머지 타입

---

### Wrapper Class

- 자바에서는 8개의 기본형을 객체로 다루지 않는데 이 기본형을 객체로 만든 것이 래퍼 클래스이다.

---

### 차이점

1. 기본 타입은 값만 가지고 있으나, 래퍼 클래스는 값에 더해 식별성이란 속성을 갖는다.

   - 래퍼 클래스는 '==' 연산자를 이용해 값을 비교할 수 없다.

2. 기본 타입의 값은 언제나 유효하나, 박싱 된 기본 타입은 유효하지 않은 값, 즉 null을 가질 수 있다.

   - SQL과 연동할 경우 null 값을 처리하기 위해 래퍼 클래스를 사용할 수 있다.

3. 기본 타입이 래퍼 클래스보다 시간과 메모리 사용면에서 더 효율적이다.

   - 래퍼 클래스는 산술 연산이 불가능하기 때문에, 산술 연산을 하기 위해서는 기본 타입으로 언박싱을 해야 한다. 

   - ```java
     Integer A = 3;
     Integer B = 2;
     Integer C = A+B;
     ```

   - 위의 코드는 C에 A와 B의 값을 더 하는 코드이다. 실제로 코드가 실행될 때 A와 B를 int로 언박싱을 하여 값을 더하고 결괏값을 Integer로 박싱하여 저장하게 된다. 이러한 추가 과정이 생긴다.

   - 기본 타입은 Stack에 값을 바로 저장하지만 래퍼 클래스는 heap 영역에 값을 저장하고 그 주소를 Stack에 저장하기 때문에 메모리를 더 사용하게 된다.

---

### 참고자료

- 자바의 정석
- 이펙티브 자바