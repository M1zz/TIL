# [인터뷰질문 005] Array와 Set의 차이가 무엇인가요?

## Array와 Set의 차이점
Array, 한국어로 하면 배열입니다. 의미 그대로 무엇인가를 나열할 수 있는 배열이기 때문에 다양한 타입이 오기도 하고 중복되는 같은 값이 와도 이상하지 않습니다.

Set, 한국어로 하면 집합입니다. 이런 저런것들이 나열되어 있다기 보다는 중복되지 않은 값들을 그룹으로 모아놓은 느낌이 강합니다.

정리 해 보면, 여러개의 값들을 나열할 수 있는 것은 `Array`, 중복을 제거한 집합은 `Set`입니다.
그리고 조회에 있어서 `Set`가 `Array`에 비해 훨씬 빠릅니다.

아래의 코드를 통해 두 차이점을 비교해 보고 눈으로 확인 해 보도록 하겠습니다

```
import Foundation

let sampleArray: [Int] = [1,1,2,3,3,3]
var sampleSet:Set<Int> = Set([1,1,2,3,3,3])

print(sampleArray) // [1, 1, 2, 3, 3, 3]
print(sampleSet) // [2, 3, 1]

var numbers:[Int] = []
while numbers.count < 10000 {
    var number = Int.random(in: 1...100000)
    if !numbers.contains(number){
        numbers.append(number)
    }
}
sampleSet = Set(numbers)
let startTime = CFAbsoluteTimeGetCurrent()
print(sampleSet.contains(98891))
let processTime = CFAbsoluteTimeGetCurrent() - startTime
print("set process time : \(processTime)") // set process time : 0.00021398067474365234

let startTime2 = CFAbsoluteTimeGetCurrent()
print(numbers.contains(98891))
let processTime2 = CFAbsoluteTimeGetCurrent() - startTime
print("array process time : \(processTime2)") // array process time : 0.000980973243713379
```
