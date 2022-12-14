# Stack(스택)

Stack이란 "어떤 것을 쌓는다"는 것을 표현하기 위해 만든 선형 자료 구조로 배열과 같지만 특정 기능에 특화된 배열이다(원소를 쌓는 것에 중점을 둔 배열)
Stack을 활용하는 곳은 무언가를 쌓고 맨 위의 것을 빼는 구조라는 것을 추상적으로 알 수 있게 된다
<br><br>
### Stack 특성
Stack의 특성 : "LIFO"
- LIFO : Last In First Out
- 마지막에 들어온 원소가 가장 먼저 나간다는 의미

택배를 쌓으면 맨 위에 있는 택배부터 꺼내야 하는 것처럼 Stack도 원소를 push하면 가장 위(뒤)에 있는 원소를 pop한다
<br><br>
### Stack 기능
- push: 원소를 추가한다
- pop: 원소를 삭제 한다
- peek(or top): 맨 위의 원소를 가져온다

Stack에 원소를 추가할때는 항상 마지막에 추가된다( O(1)의 시간복잡도로 원소를 추가할 수 있다)
Stack에서 원소를 지울 때는 항상 마지막의 원소가 삭제된다( O(1)의 시간복잡도로 원소를 추가할 수 있다)
마지막 원소를 삭제하는 것이므로 원소를 앞으로 재배치하는 오버헤드가 발생하지 않는다<br>
peek(top)도 항상 마지막 원소를 참조하므로 O(1)의 시간복잡도를 갖는다
<br><br>
### Swift로 Stack 구현하기
- push: 원소를 맨 뒤에 추가한다
- pop: 맨 뒤의 원소를 삭제하고 삭제한 원소를 return한다
- top: 맨 뒤의 원소를 return한다
- size: Stack의 현재 크기를 return한다
- isEmpty: Stack이 비었는가를 return 한다

```swift
import Foundation

struct Stack<T> {
    private var stack: [T] = []
    
    public var size: Int {
        return stack.count
    }
    
    public var isEmpty: Bool {
        return stack.isEmpty
    }
    
    public mutating func push(_ element: T) {
        stack.append(element)
    }
    
    public mutating func pop() -> T? {
        return stack.popLast()
    }
    
    public mutating func top() -> T? {
        return stack.last
    }
}

var stack = Stack<Int>()
print("1) isEmpty? \(stack.isEmpty)")

stack.push(1)
stack.push(2)
print("2) isEmpty? \(stack.isEmpty)")
print("3) stack size: \(stack.size)")
print("4) top: \(stack.top())")

stack.pop()
print("5) top: \(stack.top())")
print("6) stack size: \(stack.size)")
```

1 - true가 출력된다 생성한 직후의 Stack은 당연히 비어있다. <br>
2 - false가 출력된다 Stack에는 [1, 2] 원소가 들어 있다 <br>
3 - 2가 출력된다 <br>
4 - 2가 출력된다 맨 뒤의 원소를 출력한다 <br>
5 - 1이 출력된다 2가 pop이 되었기 때문에 맨 뒤의 원소는 1이다 <br>
6 - 1이 출력된다 남은 원소는 한 개이다 <br>