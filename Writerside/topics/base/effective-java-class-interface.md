# 클래스와 인터페이스

## 1. 클래스와 멤버의 접근 권힌을 최소화하라.
 - 모든 클래스와 멤버의 접근성을 가능한 한 좁혀야 한다.
 - 접근제한자 private, package-private, protected, public을 사용하여 정보 은닉을 통해 클래스 내부 구현을 감추고 외부로부터의 오용을 막아야 한다.
 - public 클래스의 인스턴스 필드는 되도록 public이 아니여야 한다.
   - 해당 필드의 불변식을 보장할 수 없어. 다른 곳에서 필드가 수정될 수 있다.
 - 정적 필드도 마찬가지로 public이 아니여야 하지만, 예외로 추상 개념을 ㅎ완성하는데 꼭 필요한 구성요소의 상루무나ㅕ public static final 필드로 공개해도 좋다.
 - 길이가 0이 아닌 배열은 모두 변경이 가능하기 때문에, 클래스에서 public static final 배열 필드를 두지 말아야 한다.
```Java
// 보안 허점이 존재
public static final Thing[] VALUES = { ... };

// 새로운 배열을 할당하여 반환
private static final Thing[] PRIVATE_VALUES = { ... };
public static final List<Thing> VALUES = Collections.unmodifiableList(Arrays.asList(PRIVATE_VALUES));
 ```

## 2. public 클래스에서는 public 필드가 아닌 접근자 메서드를 사용하라.
- 패키지 바깥에서 접근할 수 있는 클래스라면 접근자를 제공함으로써 내부표현 방식을 언제든지 수정할 수 있는 유연성을 얻을 수 있다.
- package-private 클래스나 private 중첩 클래스라면 데이터 필드를 노출해도 괜찮다.

```Java
// public 클래스의 필드를 직접 노출하지만 불변식을 보장하는 코드
public final class Time {
    private static final int HOURS_PER_DAY = 24;
    private static final int MINUTES_PER_HOUR = 60;

    public final int hour; // final을 통해 불변을 보장해서 그나마 안전하다.
    public final int minute;

    public Time(int hour, int minute) {
        if (hour < 0 || hour >= HOURS_PER_DAY) {
            throw new IllegalArgumentException("시간: " + hour);
        }
        if (minute < 0 || minute >= MINUTES_PER_HOUR) {
            throw new IllegalArgumentException("분: " + minute);
        }
        this.hour = hour;
        this.minute = minute;
    }
}
```

## 3. 변경 가능성을 최소화하라.
- 불변클래스의 규칙
  - 객체의 상태를 변경하는 메서드를 제공하지 않는다.
  - 클래스를 확장할 수 없도록 한다.
  - 모든 필드를 private으로 선언한다.
    - public final로 선언해도 되지만 다음 릴리스에서 내부 표현을 바꾸지 못하므로 비권장
  - 자신 외에는 내부의 가변 컴포넌트에 접근 할 수 없도록 한다.
- 불변 클래스의 장점
  - 불변 객체는 스테드가 안전하여 따로 동기화할 필요 없다.
  - 자유롭게 공유할 수 있으며 불변 객체끼리 내부 데이터를 공유 할 수 있다.
  - 다른 불변 객체들을 구성요소로 사용하면 이점이 많다.
  - 불변 객체는 그 자체로 실패 원자성을 제공한다.
- 불변 클래스의 단점
  - 값이 다르면 반드시 독립된 객체로 만들어야 한다. 가짓수가 많다면 큰 비용이 발생할 수 있다.
  - 객체의 완성하기까지의 단계가 많다면 중간 단계에서 만들어진 객체들이 모두 버려진다면 성능 문제가 불거질 수 있다.
  - 
> 불변 클래스란? 인스턴스의 내부 값을 수정할 수 없는 클래스
> 실패 원자성(failure atomicity)이란? 연산 중간에 예외가 발생하더라도 객체는 연산 전 상태를 유지하는 특성

## 4. 상속보다는 컴포지션을 사용하라
- 상속의 단점
  - 상속은 강력하지만 캡슐화를 해칠 수 있다.
  - 상위 클래스의 변경으로 하위 클래스가 사용할 수 없는 상태가 생길 수 있다.
- 컴포지션 활용
  - 기존 클래스가 새로운 클래스의 구성요소로 쓰인다는 듯에서 컴포지션이라고 한다.

## 5. 상속을 고려해 설계하고 문서화하라. 그렇지 않으면 상속을 금지하라.
- 상속용 클래스는 재정할 수 있는 메서드들을 내부적으로 어떻게 이용하는지 문서로 남겨야 한다.
- api문서 메서드 설명 끝에 종종 "Implementation Requirements"라는 절을 두어서 상속용으로 설계한 클래스의 내부 동작 방식을 명시하는 경우가 있다.
- 내부 동작 과정 중간에 끼어들 수 있는 훅(Hook)을 잘 선별하여 protected 메서드로 제공하는 것도 좋은 방법이다.
- 상속용으로 설계한 클래스는 배포 전에 반드시 하위 클래스를 만들어 검증해야 한다.
- 상속용 클래스의 생성자는 직접적으로든 간접적으로든 재정의 가능 메서드를 호출해서는 안된다.
- 상속용으로 설계한 클래스는 Cloneable과 Serializable을 구현하지 않는 것이 좋다.

## 6. 추상 클래스보다는 인터페이스를 우선하라.
- 추상클래스를 상속하는 경우 추상클래스 하위에 존재해야 하며, 한 번에 하나의 클래스만 상속받을 수 있다
- 인터페이스로 계층구조가 없는 ㅌ아비 프레임워크를 만들 수 있다.
- 인터페이스는 기능을 향상 시키고 안전하고 강력한 수단이 된다.

## 7. 인터페이스는 구현하는 쪽을 생각해 설계하라.
- default 메서드는(컴파일에 성공하더라도) 기존 구현제체 런타임 오류를 일으킬 수 있다.
- 인터페이스를 릴리스한 후라도 결함을 수정하는 게 가능한 경우도 있겠지만, 절대 그 가능성에 기대서는 안된다.

## 8. 인터페이스는 타입을 정의하는 용도로만 사용하라.


## 9. 태그 달린 클래스보다는 클래스 계층구조를 활용
- 태그 달린 클래스느 ㄴ장황하고 , 오류를 내기 쉽고 비효율적이다.
- 태그 달린 클래스는 클래스 계층 구조를 어설프게 흉내낸 구조에 불구하다.

Example
```Java
// 태그 달린 클래스 - 클래스 계층구조보다 훨씬 나쁘다.
class Figure {
    enum Shape { RECTANGLE, CIRCLE };

    // 태그 필드 - 현재 모양을 나타낸다.
    final Shape shape;

    // 모양이 사각형일 때만 사용하는 필드
    double length;
    double width;

    // 모양이 원일 때만 사용하는 필드
    double radius;

    // 원용 생성자
    Figure(double radius) {
        shape = Shape.CIRCLE;
        this.radius = radius;
    }

    // 사각형용 생성자
    Figure(double length, double width) {
        shape = Shape.RECTANGLE;
        this.length = length;
        this.width = width;
    }

    double area() {
        switch (shape) {
            case RECTANGLE:
                return length * width;
            case CIRCLE:
                return Math.PI * (radius * radius);
            default:
                throw new AssertionError();
        }
    }
}
```

```Java
// 클래스 계층구조를 활용한 클래스
abstract class Figure { // 팩토리 메서드 패턴
    abstract double area();
}

class Circle extends Figure {
    final double radius;

    Circle(double radius) {
        this.radius = radius;
    }

    @Override
    double area() {
        return Math.PI * (radius * radius);
    }
}

class Rectangle extends Figure {
    final double length;
    final double width;

    Rectangle(double length, double width) {
        this.length = length;
        this.width = width;
    }

    @Override
    double area() {
        return length * width;
    }
}

```

## 10. 멤버 클래스는 되도록 static으로 만들라.
- 비정적 멤버 클래스의 인스턴스는 바깥 클래스의 인스턴스와 암묵적으로 연결된다.
- 비정적 멤버 클래스의 인스턴스는 바깥 클래스의 참조를 갖고 있기 때문에 메모리 누수가 발생할 수 있다.
- 비정적 멤버 클래스의 인스턴스는 바깥 클래스의 참조를 갖고 있기 때문에 직렬화할 수 없다.

## 11. 톱 레벨 클래스는 한 파일에 하나만 담아라.
- 여러 톱 레벨 클래스를 한 파일에 담으면 컴파일러가 어떤 클래스를 먼저 컴파일할지 알 수 없다.
- 한 파일에 여러 톱 레벨 클래스를 담으면 소스 파일을 찾기 어려워진다.

