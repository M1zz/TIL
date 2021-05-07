[![](https://1.bp.blogspot.com/-La0_EwnwIYU/YHm7RqinmLI/AAAAAAAAivU/ifMlC7RvdVAqhMvfan--tqYVthBxbDoIQCLcBGAsYHQ/s320/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA%2B2020-09-18%2B%25E1%2584%258B%25E1%2585%25A9%25E1%2584%2592%25E1%2585%25AE%2B3.25.04.png)](https://1.bp.blogspot.com/-La0_EwnwIYU/YHm7RqinmLI/AAAAAAAAivU/ifMlC7RvdVAqhMvfan--tqYVthBxbDoIQCLcBGAsYHQ/s346/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA%2B2020-09-18%2B%25E1%2584%258B%25E1%2585%25A9%25E1%2584%2592%25E1%2585%25AE%2B3.25.04.png)

*   이 글은 제목처럼 Optional이란 무엇인가요? 라는 질문에 대답하지 못하는 사람을 위해 쓰였습니다.
*   Swift 문법 책에서 Optional은 써 보았지만, 무엇인지 왜 쓰는지 모르는 사람을 위해 쓰였습니다.
*   그리고 미래의 저를 위해 쓰였습니다.

### 옵셔널은 타입

옵셔널을 이해하기 위해서는 타입에 대한 이해가 필요합니다. 스위프트에는 두 종의 타입이 필요합니다. 하나는 이름이 있는 타입(named type)이고, 나머지 하나는 이름이 없는 혼합된 타입(compound Types)입니다. 사용자가 만든 클래스 타입, 배열, 딕셔너리 등은 전부 다 이름이 있는 타입입니다. 또한 데이터의 자료형을 나타내는 숫자, 문자 형들도 다 이름이 있는 타입이죠.

혼합 타입은 이름이 없이 정의된 타입입니다. 예를들면 튜플의 (Int, Double) 로 정의된 타입들이 혼합형 타입입니다. 이름이 없고, 이미 정의된 타입들이 섞여있습니다.

그렇다면 타입의 종류에는 어떤 것들이 있을까요?

    type → function-type
    type → array-type
    type → dictionary-type
    type → type-identifier
    type → tuple-type
    type → optional-type
    type → implicitly-unwrapped-optional-type
    type → protocol-composition-type
    type → opaque-type
    type → metatype-type
    type → any-type
    type → self-type
    type → ( type )


타입들을 살펴보다보면, 옵셔녈 타입이 있습니다. 그러므로 옵셔널은 타입중에 하나 입니다. 이름 그대로 이미 정의된 타입에 값이 있을 수도 있고 없을 수도 있는 타입입니다.

### 옵셔널로 만들어 써보기

옵셔널 타입을 만드는 방법은 간단합니다. 타입의 뒤에 `?` 기호를 붙여주거나, `Optional<T>`로 한 번 감싸주면 됩니다. 옵셔널로 만든다는 뜻은, 이 타입은 해당 타입의 값이 있을 수도, 없을 수도(nil) 있다는 뜻 입니다.

코드로 좀 더 살펴보겠습니다.

    var justInteger: Int
    var optionalInteger: Int?


justInteger라는 변수에는 반드시 정수의 값이 있어야 합니다. 하지만 optionalInteger라는 변수에 접근했을 때에는 정수가 있을 수도 nil이 있을 수도 있습니다. 접근하는 방법은 옵셔널 바인딩을 이용하거나, `!`를 변수 뒤에 붙여 강제로 값을 꺼낼 수 있습니다. 만약 강제로 꺼냈는데 nil이 들어있다면, 앱이 죽을 수 있으니 조심해야 합니다.

### Optional의 구현

간단하게 이야기 정리하면 옵셔널은 enum 입니다. 정확하게 똑같이 구현되어있지는 않지만, 아래와 같은 코드로 구현되어있습니다.

    enum Optional<Wrapped> {
      case none
      case some(Wrapped)
    }


해당 타입의 값이 있으면, some()으로 감싸 옵셔널 타입으로 반환해주고, 없으면 none 케이스에서 -> nil을 만들어 반환 해줍니다. 사용하는 입장에서 분기처리를 해서 사용하면 편리하겠죠?

### 정리

Optional이란 무엇인가요? 라는 질문에 대답이 나오지 않아 정리하게 된 글입니다.

면접에서 이런 질문을 받는다면, 옵셔널은 타입중 하나입니다. 이름이 있는 타입의 뒤에 ?를 붙여 만들 수 있는데, 해당 타입의 값이 있을 수도 있고 없을 수도 있다는 뜻의 타입입니다. enum으로 한 번 감싸져 있어서, 값이 들어있는지 옵셔널 바인딩이나, 강제 언래핑을 사용해서 값에 접근해야 하는 타입입니다. 라고 답할 것 같습니다.

혹시 답변이 이상하거나, 첨언 해주신다면 기쁘게 수정하겠습니다 의견부탁드립니다.

### 참고

*   [Swift 공식문서 Types](https://docs.swift.org/swift-book/ReferenceManual/Types.html#)
*   [Optional 구현체](https://github.com/apple/swift/blob/main/stdlib/public/core/Optional.swift)
