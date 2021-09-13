제네릭에 대해 알아보자

제네릭이라는 단어를 듣고 여러 타입을 가리키는 타입변수 라는 정의만 알고있고 실제로 제네릭을 쓰기만 하고 
구현해보지 않아
익숙하지 않은 부분이 존재하는 것 같아서 직접 함수를 구현해보면서 온전하게 이해해 보도록 하겠습니다.
설명은 애플 공식문서에 잘 나와있기 때문에 참고하였습니다.  
[문서 보러가기](https://docs.swift.org/swift-book/LanguageGuide/Generics.html#//apple_ref/doc/uid/TP40014097-CH26-XID_283)

### 제네릭이 왜 필요하게 되었지
편리한 것들은 과거에 어려움이 있었기 때문에 그 문제를 해결하기 위해서 생겨나겟 되었습니다. 
제레릭이 어떤 문제들을 해결하기 위해 생겨났는지 살펴보겠습니다.  
아래의 코드를 한번 살펴보시죠.
```
func swapTwoInts(_ a: inout Int, _ b: inout Int) {
    let temporaryA = a
    a = b
    b = temporaryA
}

var someInt = 3
var anotherInt = 107
swapTwoInts(&someInt, &anotherInt)
// someInt : 107, anotherInt : 3"
```
쉬운 예제네요. 두 변수의 주소값을 입력 해주면 두 변수의 값이 서로 바뀌어 있습니다.  
이 함수를 잘 쓰고 있다가, Double형이나 String 형도 변환할 일이 생기면 다음과 같이 추가로 구현해 주어야합니다.

```
func swapTwoStrings(_ a: inout String, _ b: inout String) {
    let temporaryA = a
    a = b
    b = temporaryA
}

func swapTwoDoubles(_ a: inout Double, _ b: inout Double) {
    let temporaryA = a
    a = b
    b = temporaryA
}
```
와... 그러면 같은 기능에 변수의 타입이 다를 경우 전부 다 구현해주어야 하는걸꺼요? 그래서 생긴 개념이 이 제네릭입니다.
그럼 어떻게 사용하고 그 형태는 어떤지 살펴보겠습니다.

### 어떻게 생겼지?
제네릭을 이용해 위의 문제를 해결한 함수는 아래와 같이 생겼습니다.
```
func swapTwoValues<T>(_ a: inout T, _ b: inout T) {
    let temporaryA = a
    a = b
    b = temporaryA
}
```
거의 똑같죠 다만 저 <T>가 함수를 정의할 때 붙이는 것을 제외하면. 저 T는 placeholder 입니다. 실제 있는 타입인지는
체크하지 않고, T라고 명시 되어있는 부분만 제네릭 타입을 적용해주죠.

### 제네릭 타입
우리가 Array, Dictionary에 어느 타입의 데이터든 넣을 수 있는 것 처럼 제네릭 타입도 사용될 수 있습니다.
코드를 통해 예를 살펴보게습니다.
```
struct IntStack {
    var items = [Int]()
    mutating func push(_ item: Int) {
        items.append(item)
    }
    mutating func pop() -> Int {
        return items.removeLast()
    }
}

var stack = IntStack()
stack.push(3)
stack.push(5)
print(stack.items)
// [3, 5]
```

IntStack 이라는 스트럭트는 Int를 하나 받아서 push 하거나 pull 할 수 있습니다.
이러면 위에서 생겼던 문제와 같은 문제들이 생길 수 있습니다. String을 넣거나, Double할 때마다 새로 구현을 해주어야 하기 때문이죠.
그렇다면? 제네릭을 쓰면 해결될 것 같은데, 어떻게 쓸 수 있을지 알아보겠습니다.
```
struct Stack<Element> {
    var items = [Element]()
    mutating func push(_ item: Element) {
        items.append(item)
    }
    mutating func pop() -> Element {
        return items.removeLast()
    }
}

var stack = Stack<Int>()
stack.push(4)
stack.push(7)
print(stack.items)
```

### 그럼 항상 모든 타입이 올 수 있단 말인가요?
그렇지 않습니다 당연히 타입의 제약을 둘 수 있습니다. 특정 클래스의 하위 클래스여야 한다는 제약과
프로토콜을 준수하여야 한다는 제약을 걸 수 있습니다.  

아래의 코드를 예들들어 보겠습니다.

```
func findIndex(ofString valueToFind: String, in array: [String]) -> Int? {
    for (index, value) in array.enumerated() {
        if value == valueToFind {
            return index
        }
    }
    return nil
}
```
String 배열과 찾을 문자열을 입력하면, 해당 문자열의 인덱스를 반환 해주는 함수입니다. 위의 예제랑 마찬가지로
Int 배열이 있고, 찾을 Int를 넣으면 그 인덱스를 반환해주는 함수가 필요해졌다면 어떻게 해야할까요?  
위의 예제와 마찬가지로 Generic을 이용해 구현해 보겠습니다.

```
func findIndex<T>(of valueToFind: T, in array:[T]) -> Int? {
    for (index, value) in array.enumerated() {
        if value == valueToFind {
            return index
        }
    }
    return nil
}
```
위의 코드처럼 바꾸면, `Binary operator '==' cannot be applied to two 'T' operands` 이라는 에러메세지를 마주하게 됩니다.  
모든 타입의 동치비교를 `==`연산자로 할 수 있지 않기 때문이죠. 그렇기 때문에 ==를 이용해서 같음을 비교할 수 있는 타입만 받으려합니다.  
즉 제약을 걸어주는 것이지요.

```
func findIndex<T:Equatable>(of valueToFind: T, in array:[T]) -> Int? {
    for (index, value) in array.enumerated() {
        if value == valueToFind {
            return index
        }
    }
    return nil
}
```
위와 같이 `Equatable`을 채택해서 제약을 걸어주면 동치 비교를 할 수 있습니다.



