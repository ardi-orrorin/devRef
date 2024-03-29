# Refactoring

## 1. 작명(Mysterious Name)
- 함수, 변수, 클래스, 패키지, 소스 파일 등의 이름을 지을 때는 그 목적을 쉽게 이해할 수 있도록 지어야 한다.
- 이름을 지을 때는 일관성이 있어야 한다.
- 메소드 이름은 동사로 시작하고, 클래스 이름은 명사로 시작한다.
- 필드 이름은 명사로 시작하고, 상수 이름은 대문자로 시작한다.
- 패키지 이름은 소문자로 시작하고, 도메인 이름을 역순으로 사용한다.
- 인터페이스 이름은 접미사로 -able, -ible을 사용한다.
- 인터페이스 구현 클래스는 인터페이스 이름에 Impl을 붙인다.

## 2. 중복코드(Duplicated Code)
- 코드가 중복 될 경우 서로간에 차이점이 있는 지 신경써야 하기 대문에, **함수 추출하기** 를 통해 중복을 제거한다.  
- 또한 수정시 중복된 코드를 모두 수정해야 하기 때문에, 이로 인한 버그 및 오류가 발생할 수 있다.  
- 다만, 가독성 측면에서는 중복된 코드가 더 좋을 수 있다.  
- 이러한 이유로 적절한 기준을 정하여 중복코드를 허용하는 도움될 수 있다.

## 3. 긴 함수(Long Method/Function)
- 함수가 길어지면, 함수의 목적을 이해하기 어렵다. 이상적은 함수는 하나의 기능 하나의 메소드(함수) 이다.
- 함수의 이름을 잘 지어야 함수의 의도를 알고 내용을 살펴보지 않고, 로직에 집중 할 수  있다.
- 함수의 이름은 동작 방식이 아닌 의도가 드러나도록 적는다.
- 이로 인해 함수의 내용이 길이가 길어질 수도 있다.
- 함수추출하기를 적극적으로 이용하는 것을 권장한다.
- 반복문이 여러개 있을 경우, 반복문을 함수로 추출하고, 추출한 함수를 함수로 추출한다.

## 4. 긴 매개변수 목록(Long Parameter List
- 매개변수가 많을 경우, 함수의 목적을 이해하기 어렵다.
- 데이터 구조에서 값들을 뽑아 전달해야 할 경우 객체 하나로 전달하는 것이 좋다.

## 5. 전역 데이터(Global Data)
- 전역 데이터는 코드베이스 어디에서든 건드릴 수 있지만, 그 값을 누가 바꿨는지 찾아낼 메커니즘이 없다는 점이 문제다.
- 전역 데이터의 대표적인 형태는 전역 변수지만 클래스 변수와 싱글턴도 같은 문제가 발생한다.
- 이를 방지 하기 위한 방법으로 변수 **캡슐화하기**를 사용한다.

## 6. 가변 데이터(Mutable Data)
- 가변 데이터는 코드의 다른 곳에서는 다른 값을 기대한다는 사실을 모른 채 코드를 수정해버리면 문제가 생긴다.
- 이런 이유로 함수형 프로그래밍에서는 데이터를 변경하려고 하면 해당 하는 복사본을 만들어서 반환한다는 원칙을 따른다.
- 이를 방지하기 위한 방법으로 변수 **캡슐화하기**를 적용하어ㅕ 정해놓은 함수를 거쳐야만 값을 수정할 수 있도록 하면 감시허가너 코드 개선하기 쉽다.
- 하나의 변수의 용도가 다른 값들을 저장하느라 값이 갱신되는 경우라면 **변수 쪼개기**를 이용하여 용도별로 독립 변수에 저장하는 것이 문제을 해결할 수 있다.
- 여러 함수를 클래스로 묶거나 여러 함수를 변환 함수로 묶는 방법을 통해 변수의 갱신을 제한 한다.

## 7. 확산적 변경(Divergent Change)
- 하나의 모듈이 서로 다른 이유들로 변경되는 일이 많을 때 확산적 변경이라고 한다.
- 대부분 단일 책임 원칙을(Single Responsibility Principle)이 제대로 지켜지지 않을 때 발생한다.

> 단일 책임 원칙(SRP) : 단일 모듈은 변경의 이유가 오직 하나여야만 한다.는 원칙이다.

## 8. 산탄총 수술(Shotgun Surgery)
- 확산적 변경과 비슷하지만 반대의 개념이다.
- 하나의 변경을 위해 여러 모듈을 수정해야 할 때 발생한다.
- 이를 방지하기 위해 함수옮기기, 필드 옮기기, 여러 함스를 클래스로 묶기 처럼 비슷한 데이터를 다루는 내용들은 묶어서 관리해야 한다.

## 9. 기능 편애(Feature Envy)
- 모듈화할 때, 코드를 여러 영역으로 나눈 뒤, 영역 안에서의 상호작용은 최대한 늘리고, 영역간의 상호작용은 최소한으로 줄이는데 주력한다.
- 어떤 함수가 자신이 속한 모듈의 데이터보다 다른 모듈의 데이터와 더 많이 상호작용할 때 기능 편애라고 한다.
- 이런 경우 더 많이 상호작용하는 모듈로 옮기는 것이 좋다.

## 10. 데이터 뭉치(Data Clumps)
- 여러 함수 및 클래스에서 같은 데이터들을 사용할 경우, 데이터들을 묶어서 관리하는 것이 좋다.
- 데이터 뭉치를 판별하는 방법은 같이 다니는 데이터 중 하나를 삭제해서 다른 데이터들이 의미를 잃는지 확인하는 것이다.

## 11. 기본형 집착
- 기본형을 사용하면 데이터를 묶기가 어렵다.
- 기본형을 클래스로 바꾸기를 사용하여 기본형을 클래스로 바꾸고, 기본형을 감싸는 클래스 및 객체로 관리한다

## 12. 반복되는 switch문
- switch문은 다형성을 사용하여 제거한다.
- 단, switch문을 제거하기 위해 다형성을 사용하는 것이 아닌, switch문에 조건절 같은 내용이 추가 될 때 다른 switch문도 같이 수정되야하는 문제가 발생된다면, 다형성을 추천한다.

## 13. 반복문(Loops)
- 함수를 통해 반복문을 제거하는 것이 좋다.
- 여기서 함수란 파이프라인 역할을 할 수 있는 함수로, 컬렉션 파이프라인, 재귀 함수 등을 의미한다.
- 예를 들어 자바의 경우 stream()을 사용하면 반복문이 역할을 할 수 있는 forEach map filter 등의 함수를 사용할 수 있다.

## 14. 성의 없는 요소(Lazy Element)
- 시간이 지나 쓸모 없어진 클래스의 메소드들의 삭제로, 역할이 적어진 요소가 있다면, 클래스, 함수 인라인하기를 삭제하여 처리하거나, 상속 받은 개체가 있다면, 계층합치기를 적용한다.

## 15. 추측성 일반화(Speculative Generality)
- 추측성 일반화란, 미래에 필요할 것 같은 기능을 추가하는 것이다.
- 하지만 미래에 사용하게 되면 다행이라는 말이 있을 정도로 사용 여부가 불투명하기 때문에 사용할 때 코드를 작성하는 것이 좋다.
- 하는 일이 없는 추상클래스는 삭제하고, 추상클래스를 상속받은 클래스들을 삭제한다.

## 16. 임시 필드(Temporary Field)
- 간혹 특정 상황에서만 사용하는 필드를 임시 필드라고 한다.
- 개발자는 클래스에서 모든 필드들이 항상 사용된다는 기대하는데, 이렇게 임시 필드가 존재 않다면, 이 필드의 역할을 찾기위해 시간을 낭비하게 된다.
- 이를 방지하기 위해, 메소드 변수나, 아예 클래스를 새로 빼서 관리하는 것이 좋다.

## 17. 메시지 체인(Message Chains)
- 한 객체를 통해 다른 객체를 호출하고, 그 값이 다시 다른 객체를 호출하는 식으로 연쇄적 호출이 일어나는 코드를 메시지 체인이라고 한다.
- 이는 중간단계의 코드를 수정하게 되면 결과 값이 나비효과처럼 크게 달라질 수 있다..


## 18. 중개자(Middle Man)
- 객체에 대표적인 기능으로, 캡슐화가 있는데, 이 과정에서 위임이 자주 활용된다.
- 클래스가 제공한느 메서드 중 절반이 다른 클래스에서 구현을 위임하면, 중개자가 되는 클래스는 제거하는 것이 좋다.

## 19. 내부자 거래(Insider Trading)
- 

## 20. 거대한 클래스(Large Class)
- 클래스가 너무 많은 일을 하거나, 너무 많은 데이터를 가지고 있게되면, 중복코드가 발생할 확률이 높아진다.
- 또한 필드들 모두가 항시 사용하지 않을 수도 있기 때문에 임시필드가 발생할 확률이 높아진다.
- 슈퍼클래스 추출, 서브클레스 추출하기 등을 통해 클래스를 분리한다.


## 21. 서로 다른 인터페이스의 대안 클래스(Alternative Classes with Different Interfaces)
- 클래스의 큰 장점 중 하나는 필요에 따라 언제든 다른 클래스로 교체 할 수 있다는 것이다.
- 단, 교체하려면 인터페이스나 슈퍼클래스가 같아야 한다.

## 22. 데이터 클래스(Data Class)
- 데이터 클래스란, 필드와 게터, 세터만 있는 클래스를 말한다.
- 불변 필드는 굳이 캡슐화를 할 필요가 없다. 그러니 getter를 통하지 않고 공개해도 된다.
- 

## 23. 상속 포기(Refused Bequest)
- 상속 포기란, 상속받은 메소드를 사용하지 않는 경우를 말한다.
- 과거에는 이런 현상을 방지하기 위해 추상클래스를 권장했지만, 요새는 상속 포기도 괜찮다고 한다.

## 24. 주석
- 뭘 할 지 모를 때 
- 지금 처럼 코드를 작성한 이유를 설명할 때


