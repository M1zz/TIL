# [인터뷰질문 025] CaseIterable 프로토콜은 어떤 역할을 합니까?

관련주제 : Swift
난이도 : 하

enum의 case들을 전부 순회할 수 있게 만들어 줄 때 채택하는 프로토콜 입니다. 예들들어

아래와 같은 코드를 예로 들어보겠습니다.

```
enum Fruits {
    case melon
    case banana
    case kiwi
    case apple
}
```

이렇게 내가 가진 과일들에 선택지가 제한이 있다면, enum을 써서 정의 해 놓을 수 이습니다. 이 때, 모든 케이스에 해당히는, 순회가 있어야 한다면 `CaseIterable`를 채택해서 `allCases`를 사용할 수 있습니다.

그렇게 되었을 때, 코드를 작성해 보겠습니다.

```
enum Fruits: CaseIterable {
    case melon
    case banana
    case kiwi
    case apple

    var name: String {
        switch self {
        case .apple:
            return "사과"
        case .melon:
            return "멜론"
        case .banana:
            return "바나나"
        case .kiwi:
            return "키위"
        }
    }
}

let fruitNames = Fruits.allCases.map { $0.name }
print(fruitNames) // ["멜론", "바나나", "키위", "사과"]
```

심지어, enum에 정의한 순서대로, 순회하기 때문에 순서도 보장됩니다!

## 정리

정리해 보자면, 우리는 enum을 사용하여 우리의 선택지를 제한할 수 있습니다. 그리고 그 제한된 선택지를 모두 순회해서 새로운 형태의 데이터를 만드는데 유용합니다.

개인적인 느낌에 map과 같이 사용했을 때 정말 강력한 기능을 발휘하는 것 처럼 느껴져서 이 부분은 조금 더 정리해 두어야겠습니다.
