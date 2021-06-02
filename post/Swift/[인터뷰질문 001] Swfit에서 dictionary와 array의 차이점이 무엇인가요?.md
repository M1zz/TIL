# [인터뷰질문 001] Swfit에서 dictionary와 array의 차이점이 무엇인가요?

![arraydictionary](https://docs.swift.org/swift-book/_images/CollectionTypes_intro_2x.png)
## 데이터 접근 방식의 차이
array는 정렬된 값들의 모임이고, dictionary는 정렬되지 않은 key-value 쌍의 값들의 모임입니다.
그러므로 데이터에 접근하는 방법이 다릅니다.

```
array - 숫자로 된 index에 데이터에 접근합니다.
// print(myArray[3])
dictionary - 사용자가 정의 한 key값을 통해 value에 접근합니다.
// print(myDictionary["apple"])
```

## 데이터 정렬의 차이
array는 index의 순서대로 정렬이 되어있습니다. 그래서 반복문으로 출력을 하게 되면 일정한 순서가 있고, 순서대로 출력되게 됩니다.
```
let fruits = ["banana", "apple", "avocado"]
for item in fruits {
    print(item)
}
// banana
// apple
// avocado
```

물론 정렬을 지원하기도 합니다.

```
let sortedFruits = fruits.sorted {$0 < $1}
for item in sortedFruits {
    print(item)
}
// apple
// avocado
// banana
```

dictionary는 기본적으로 정렬되어있지 않습니다. 이게 무슨뜻 인지 아래 코드로 설명하겠습니다.
```
let fruits = ["banana":"$3", "apple":"$8", "avocado":"$12"]
for item in fruits {
    print(item.key, item.value)
}
// avocado $12
// banana $3
// apple $8
```
출력된 결과를 보면, 알파벳 순도 아니고, 그 어떤 기준도 없습니다. 정의 그대로 정렬되어 있지 않은 키-값 쌍의 값들 입니다.
하지만 정렬이 불가능한 것은 아닙니다.

```
let sortedFruits = fruits.sorted {$0 < $1}
for item in sortedFruits {
    print(item.key, item.value)
}
// apple $8
// avocado $12
// banana $3
```

## 정리
간단한 질문이고 바로 대답할 수 있을 줄 알았는데, 실제 예시를 작성하면서 정리하니 생각보다 설명해야하는 부분이 좀 되는 것 같았습니다. 기초적인 부분이라고 생각되니 질문을 보고 바로 생각이 나지 않을 때 보면 좋을 것 같습니다.
