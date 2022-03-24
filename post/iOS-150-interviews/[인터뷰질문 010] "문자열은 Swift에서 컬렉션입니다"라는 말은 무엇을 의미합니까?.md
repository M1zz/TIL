# [인터뷰질문 010] "문자열은 Swift에서 컬렉션입니다"라는 말은 무엇을 의미합니까?

관련주제 : Swift
난이도 : 하

`String type`은 `Collection` 프로토콜을 채택했기 때문에, 문자열은 컬렉션입니다. 라고 말할 수 있습니다. 이것이 의미하는 바는, `Collection`타입이 가지고 있는 프로퍼티와 메소드들을 사용할 수 있습니다. 예를들면 `startIndex`, `isEmpty`와 같은 프로퍼티, `index(_ i: Self.Index, offsetBy distance: Int, limitedBy limit: Self.Index) -> Self.Index?`와 같은 메소드 입니다.

`Collection`프로토콜은 원소들을 순회하는 연속성을 가지고 있습니다. 기본적으로 `Sequence`프로토콜을 채택하고 있기 때문입니다. 하지만 `Sequence`프로토콜과 다른점은, 순서대로만이 아닌 임의 key, index 값을 가지고 접근할수 있습니다. 또한 처음부터 끝까지 돌아가는게 아닌, 중간부분부터 얻을 수 있습니다.
