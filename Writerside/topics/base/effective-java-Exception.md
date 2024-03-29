# 예외

## 1. 예외는 진짜 예외 상황에서만 사용하라.
- 예외는 오직 예외 사항에서만 써야하며, 절대로 인상적인 제어 흐름용으로 쓰여선 안 된다.
- 잘 설계된 API라면 클라이언트가 정상적인 제어 흐름에서 예외를 사용할 일이 없게 해야 한다.

## 2. 복구할 수 있는 상황에는 검사 예외를, 프로그래밍 오류에는 런타임 에외를 사용하라.
- 호출하는 쪽에서 복구하리라 여겨지는 상황이라면 검사 예외를 사용하라.
- 프로그래밍 오류라면 런타임 예외를 사용하라.
- 구현하는 비검사 thorwable은 모두 RuntimeException의 하위 클래스여야 한다.

## 3. 필요 없는 검사 예외 사용은 피하라.
- 검사 예외는 프로그래머에게 부가적인 부담을 주기 때문에 남용해서는 안 된다.
- 검사 예외를 던지게 되면 클라이언트는 그 예외를 catch 블록으로 잡아 처리하거나, 그 메서드를 자신이 다시 던질 수 있어야 한다.

## 4. 표존 예외를 사용하라.
- Exception, RuntimeException, Throwable, Error를 직접 잡아서는 안 된다.
- 널리 사용하는 예외 목록

| 예외 | 사용처 |
|---|---|
| IllegalArgumentException | 허용하지 않는 값이 인수로 넘어왔을 때 |
| IllegalStateException | 객체가 메서드를 수행하기에 적절하지 않은 상태일 때 |
| NullPointerException | null을 허용하지 않는 메서드에 null을 건낼 때 |
| IndexOutOfBoundsException | 인덱스의 범위를 넘어섰을 때 |
| ConcurrentModificationException | 허용하지 않는 동시 수정이 발생했을 때 |
| UnsupportedOperationException | 호출한 메서드를 지원하지 않을 때 |

## 5. 추상화 수준에 맞는 예외를 던지라.
- 상위 계층에서는 저수준 예외르 잡아 자신의 추상화 수준에 맞는 예외로 바꿔 던져야 한다. (예외 번역)
- 예외를 번역할 때 , 저수준 예외가 디버깅에 도움이 된다면 예왼 연쇄(exception chaining)를 사용하자.
- 무턱대고 예외를 전파하는 것보다야 예외 변역이 우수한 방법이지만, 그렇다고 남용해서는 안 된다.
- 

```Java
// 예외 번역
try {
    ... // 저수준 추상화를 이용한다.
} catch (LowerLevelException e) {
    throw new HigherLevelException(...);
}

// 예외 연쇄
try {
    ... // 저수준 추상화를 이용한다.
} catch (LowerLevelException e) {
    throw new HigherLevelException(..., e);
}
```

## 6. 메서드가 던지는 모든 예외를 문서화하라.
- 검사 예외는 항상 따로따로 선언하고, 각 예외가 발생하는 상황은 자바독의 @throws 태그를 사용해 정확히 문서화하자.
- 메서드가 던질 수 있는 예외를 각각 @thorows 태그로 문서화 하되, 비검사 예외는 메서드 선언의 throws 목록에 넣지 말자.
- 한 클래스에 정의된 많은 메서드가 같은 이유를 같은 예외를 던진다면 그 에외를 클래스 설명에 추가하는 방법도 있다.

## 7. 에외의 상세 메시지에 실패 관련 정보를 담아라.
- 실패 순간을 포착하려면 발생한 예외에 관연된 모든 매개변수와 필드의 값을 실패 메시지에 담아야 한다.
- 상세 메시지에 비밀번호나 암호키 같은 정보까지 담아서는 안된다.

## 8. 실패는 원자적으로 만들어라.
- 호출된 메서드가 실패하더라도 해당 객체는 메서드 호출 전 상태를 유지해야 한다.

## 9. 예외를 무시하지 말자.
- catch블록을 비워두면 예외가 존재할 이유가 없어진다.
- 예외를 무시하기로 했다면 catch 블록 안에 그렇게 결정한 이유을 주석으로 남기고 예외 변수의 이름도 ingored로 바꾸자.

```Java
try {
    ... // 예외가 발생할 수 있는 코드
} catch (SomeException e) {
    // 무시하고 싶은 이유를 주석으로 남긴다.
    ignoredException.printStackTrace();
}
```