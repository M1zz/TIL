swift에서 데이터를 저장하는 곳을 변수라고 하는데, 이 변수에 아무것도 들어있지 않은 상태를 표현해야 할 일이 있습니다.
0이 아니고 빈 문자열("")도 아닌 아무것도 없는 것.  

변수에 아무것도 할당 되어있지 않은 것을 스위프트에서는 참을 수 없으니까요! 아무것도 들어있지 않다라는 것이라도 넣어 둡시다.  
이때 사용되는 것이, 없다 라는 오브젝트인 nil입니다. 변수에 아무 값도 들어있지 않다는 뜻 입니다.

### 사용 법

정의 타입 뒤에 ?를 붙여주면 옵셔널타입 이라는 뜻 입니다.  
```
var name: String? = "leeo"
print(name!) // leeo
name = nil
print(name!) // error
```
이렇게 사용가능합니다.

조금 더 자세히 살펴보면  

```
var name: String? = "leeo"
```

이 코드에서 타입이 String?임을 알 수 있습니다.  
정확히는 이런 뜻 입니다.  
```
var name: Optional<String>
```
왜냐하면
```
typealias String? = Optional<String>
var name: String?
```
이렇게 한 것과 같은 의미기 때문이죠.  

그렇다면 [Optional](https://github.com/apple/swift/blob/main/stdlib/public/core/Optional.swift)을
 까보고 똑같이 구현해 보도록 하겠습니다.

```
public enum CustomOptional<Wrapped>: ExpressibleByNilLiteral {
    case none
    case some(Wrapped)
    
    public init(nilLiteral: ()) { self = .none }
    public init(_ some: Wrapped) { self = .some(some) }
}

extension CustomOptional: CustomDebugStringConvertible {
  public var debugDescription: String {
    switch self {
    case .some(let value):
      var result = "Optional("
      debugPrint(value, terminator: "", to: &result)
      result += ")"
      return result
    case .none:
      return "닐"
    }
  }
}
```
우리는 ? 를 쓸 수 없기 때문에 $를 사용해서 우리만의 옵셔널을 만들어 보겠습니다.  
이제부터 Int$는 제가 만든 옵셔널 입니다.
```
typealias Int$ = CustomOptional<Int>

var test:Int$ = Int$(3)
print(test.test.debugDescription) // Optional(3)
test = nil
print(test.debugDescription) // 닐
```

옵셔널을 공부하면서 생긴 질문들과 그 해결법에 대한것들  몇가지 있습니다.
1. Int$ 타입인데 nil은 어떻게 대입할 수 있는가?
  - 옵셔널에서 채택한 `ExpressibleByNilLiteral` 프로토콜의 내용입니다. ()를 입력 받으면 none 케이스가 됩니다.
  - 그리고 CustomDebugStringConvertible 프로토콜을 채택 해 출력할 때는 "닐" 이라는 문자열로 반환합니다.


2. Int? 타입에 3을 입력할 때 Int?(3)이 아니라 3이 어떻게 가능한 것인가?
  - var number: Int = Int(3) 을 입력해야 생성되는게 맞다 이를 위한 프로토콜인 ExpressibleByIntegerLiteral이 있습니0.
  - ```
    struct IInt: ExpressibleByIntegerLiteral {
        typealias IntegerLiteralType = Int
        var value: Int
        init(integerLiteral value: IntegerLiteralType) {
          self.value = value
        }
    }
    ```
  - 옵셔널도 비슷한 프로토콜이 있지 않을까 싶네요. 알고 계시는 분은 댓글부탁드립니다!

3. 특정 연산자를 정의해서 쓴다면 옵셔널에 안전하게 타입을 하나 더 정의할 수는 없을까?
  - 크래시를 내고 싶지 않아서 쓰는 옵셔널인데 !를 사용했을 때 크래시가 나잖아요 ㅠㅠ
  - ```
  infix operator ?!: NilCoalescingPrecedence
  public func ?!<A>(lhs: A?, rhs: Error) throws -> A {
      guard let value = lhs else {
          throw rhs
      }
      return value
  }
  ```
  - 이미 이런 내용이 있었네요. [참고자료](https://www.objc.io/blog/2019/02/26/from-optionals-to-errors/)


### 정리
옵셔널은 열거형으로 되어있습니다. 라는 간단한 내용의 설명이 직접 구현해 볼 수 있을 것 이라는 자신감을 불어넣어 주었는데요, 생각보다
고려해야 할 부분이 많아서 어려웠습니다.  
하지만 공부하면서 어떻게 구현되어있는지 알 수 있어서 많은 도움이 되었습니다. 여러분도 여러분만의 타입을 구현해 보시고!  
제가 하지 못했던 연산자까지 정의해 보시기 바랍니다!

