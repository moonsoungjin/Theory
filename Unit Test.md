 # 유닛 테스트(Unit Test)

## 유닛테스트란?
- 유닛 테스트는 컴퓨터 프로그래밍에서 소스 코드의 특정 모듈이 의도된 대로 정확히 작동하는지 검증하는 절차다
- 모든 함수와 메소드에 대한 테스트 케이스(Testcase)를 작성하는 절차를 말한다
- unit test는 작성한 프로그램이 의도한 대로 동작하는지 검증하는 가장 작은 단위테스트 이다
- 이를 통해 각 모듈(클래스, 메소드)들이 잘 동작하는지 확인할 수 있다

## 왜 필요할까?
- 각각의 모듈을 부분적으로 확인할 수 있어 어떤 모듈에서 문제가 발생하는지 빠른 확인이 가능
- 전체 프로그램을 빌드하는 대신 유닛 단위로 빌드해 확인하므로 시간 절약

## 어떻게 작성할까?
- 테스트 케이스는 서로 분리되어야 하므로(isolated), 가짜 객체 mock object(Stub)를 만들어 테스트 하는 것이 좋다고 한다
- 커플링이 걸려있는 곳은 이를 해체해 각각 분리하는 것이 좋다

## 의존성 주입
- 테스트 코드를 작성하기 위해서는 기존 코드에 의존성을 깨는 것부터 시작해야한다
- 종속성이 감소하면 수정에 민감하지 않고, 유연성과 확장성이 높아져 테스트에 용이 해진다

### 의존성(Dependency)
- 의존성은 함수에 필요한 클래스나 참조 변수에 의존하는 것을 의미한다
```
class A{
    var num: Int = 1
}

class B{
    var internalVariable = A()
}

let b = B()
print(b.internalVariable.num) // 1
```

- B클래스는 A클래스를 내부에 변수로 사용하고 있다(B->A 의존성)
- 객체끼리 의존하는 경우 많은 문제가 야기 된다
- 만일 A클래스에 문제가 생기면 B클래스에도 문제가 생길수 있어 재사용성이 낮아진다(재사용이 가능한건 상위 클래스인 A뿐이다)

### 주입(Injection)
- 내부가 아닌 외부에서 객체를 생성해서 넣어주는것

```
class A{
    var num: Int

    init(num : Int){
        self.num = num
    }

    func setNum(num: Int){
        self.num = num
    }
}

let a = A(Int(3)) // 외부에서 객체 생성
print(a.num)

a.setNum(Int(5)) // 객체 생성
```

- 외부에서 Int를 생성해 A클래스에 주입

### 의존성 주입(Dependency Injection)
- 의존성 + 주입 용어가 합쳐져 내부에서 만든 객체를 외부에서 넣어 의존성을 주입할 수 있다

```
class A{
    var aNum: Int = 1
}
class B{
    var bNum: A
    init(num: A){
        self.bNum = num
    }
}
let b = B(A())
```
- 클래스의 생성에서 의존성을 주입(외부에서 의존성을 주입하는것 만으론 의존성 주입이라도 부르지 않는다)
- 의존성 주입에서 의존성 분리는 의존 관계 역전 법칙으로 의존 관계를 분리시켜야 한다

## 의존 관계 역전 법칙(DIP: dependency inversion principle)
- 객체 지향 프로그래밍의 SOLID 원칙 중의 하나이다
- 의존 관계 법칙은 상위 계층(정책 결정)이 하위 계층(세부 사항)에 의존하는 전통적인 의존관계를 반전시킴으로써 상위 계층이 하위 계층의 구현으로부터 독립되게 할 수 있는 구조를 말한다
- 상위 모듈은 하위 모듈에 의존해서는 안된다, 상위 모듈과 하위 모듈 모두 추상화에 의존해야 한다
- 추상화는 세부 사항에 의존해서는 안된다, 세부사항이 추상황에 의존해야 한다
```
protocol DIInterface: AnyObject{
    var num: Int{get set}
}
class A: DIInterface{
    var aNum = 1
}
class B{
    var bNum: DIInterface
    init(num: DIInterface){
        self.bNum = num
    }
}

let b = B(A())
```
- A(상위모듈),B(하위모듈) 모두 DIInterface(추상화 프로토콜)에 의존해 있어 의존 관계를 독립 시킨 상태이다
- 이렇게 의존의 방향이 역전되어 제어가 반전되는 상황을 제어의 반전(IoC: Inversion of Control)이라고 표현한다

## 테스트코드 작성하기
- 프로그램을 처음 만들때 include Unit Test를 체크하거나, 기존 프로젝트에서 추가하고 싶은 경우에는 Command 6 >+ 버튼 > New Unit Test Target을 누르면 Test를 작성할 수 있는 파일이 만들어진다
```
@testable import 프로젝트이름
```
- 테스트 코드를 작성할 때 가장 먼저 이 구문을 작성해야 한다
- 기존 프로젝트의 코드에 접근할 수 있게 해주는 코드이다

```
import XCTest
@testable import 프로젝트이름

class NewTests: XCTestCase {
    override func setUpWitError() throw {
        // Put setup code here. This method is called before the invocation of each test method in the class.
    }

    override func tearDownWithError() throws {
        // Put teardown code here. This method is called after the invocation of each test method in the class.

    }
}
```
unit test 추가 시 자동으로 생성되는 코드이다
- setUpWithError: 테스트 메소드가 실행되기 전 모든 상태를 reset한다(초기화 코드)
- tearDownWithError: 테스트 동작이 끝난 후 모든 상태를 clean up 한다(해체 코드)

테스트 메소드(테스트 메소드는 반드시 test 키워드로 시작해야 한다)

```
func testScoreIsComputedWhenGuessIsHigherThanTarget() {
    // given
    let guess = sut.targetValue + 5

    // when
    sut.check(guess: guess)

    // then
    XCTAssertEqual(sut.scoreRound, 95, "Score computed from guess is wrong")
}
```
테스트 포켓은 given-when-then 구조로 작성하는것이 좋다
위 테스트 코드는 guessValue가 targetValue보다 클 경우(given) check 메소드를 테스트(when)한다
- given: 필요한 vlaue들을 셋팅
- when: 테스트 코드 실행
- then: 결과 확인(출력)

