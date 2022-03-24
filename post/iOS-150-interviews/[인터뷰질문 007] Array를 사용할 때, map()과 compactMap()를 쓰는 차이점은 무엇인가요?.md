# [인터뷰질문 007] Array를 사용할 때, map()과 compactMap()를 쓰는 차이점은 무엇인가요?

관련주제 : Data
난이도 : 하

## map vs compactMap
기본적으로 정의를 알아야 하는 내용이기 때문에, [map](https://developer.apple.com/documentation/swift/sequence/3018373-map)와 [compactMap](https://developer.apple.com/documentation/swift/sequence/2950916-compactmap)의 내용을 보고 기능을 아는 것이 중요할 것 같습니다.

짧게 요약하면, map은 내가 가진 데이터들을 다른 형태나 다른 값으로 변환 해주는 녀석 입니다.  
compactMap은 내가 가진 데이터들 중에 nil 값을 제거해 주는 녀석이죠.

용도에 맞게 쓰면 되겠습니다 아래 예제 코드를 간단히 비교해서 언제 써야 할 지 고민해보아요.

## map
```
let numberList = [1, 3, 4, 5, 7]
print(numberList) // [1, 3, 4, 5, 7]
let doubleNumberList = numberList.map { $0 * 2 }
print(doubleNumberList) // [2, 6, 8, 10, 14]
```
정말 간단한 예제입니다. 내가 가진 데이터가 있는데, 모두 2배를 해야한다면 반복문인 `for`로 모두 순회 하면서 2배씩 곱하고 그 값을 새로운 배열에 저장하는 방법이 가장 먼저 떠오르지만 위의 예제를 사용하면 `우아하게` 데이터를 변형할 수 있습니다.

## compactMap
```
let numberListWithNil: [Int?] = [1, 2, 4, nil, 1, 3]
print(numberListWithNil) // [Optional(1), Optional(2), Optional(4), nil, Optional(1), Optional(3)]
let doubleNumberListWithNil: [Int] = numberListWithNil.compactMap { $0 }
print(doubleNumberListWithNil) // [1, 2, 4, 1, 3]
```
마찬 가지로 가지고 있는 Array에 값이 항상 있다는 보장이 되지 않는 Optional 이라면, compactMap을 사용해서 Optional값을 없앤 새로운 데이터를 만들 수 있습니다.

## 응용
```
let numberListWithNil: [Int?] = [1, 2, 4, nil, 1, 3]
print(numberListWithNil)
let doubleNumberListWithNil: [Int] = numberListWithNil
    .compactMap { $0 }
    .map { $0 * 2 }
print(doubleNumberListWithNil)
```
당연히 데이터 가공의 프로세스를 만들 수 있으며, 위와 같이 먼저 nil 값을 제거 한 다음에 2배를 하는 것도 가능합니다.


조금 더 응용하면 다음과 같은 것들도 할 수 있습니다.
```
let numberListWithNil: [String] = ["5", "2", "a", "1"]
print(numberListWithNil) // ["5", "2", "a", "1"]
let doubleNumberListWithNil = numberListWithNil
    .compactMap { Int($0) }
    .map { $0 * 2 }
print(doubleNumberListWithNil) // [10, 4, 2]
```
위와 같이 문자열을 입력 받아 숫자이면, 두배하고 아니면 버리는 로직을 만들 수 도 있습니다.

추가로 FlatMap도 있으니 찾아보면 좋을 것 같습니다 :)
