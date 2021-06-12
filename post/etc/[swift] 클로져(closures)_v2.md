
함수라고 알고 있던 클로져를 공부하면서 기록합니다.  
어느 순간 이해가 안되는 코드들이 클로져로 이루어져있다는 것을 알게된다음 정리와 적용과정이 필요하다는 것을 알고 정리해 놓습니다.

### 클로저란?
[클로저](https://docs.swift.org/swift-book/LanguageGuide/Closures.html)는 코드에 전달되어 사용할 수있는 독립적인 기능 블록입니다.  
Swift의 클로저는 C 및 Objective-C의 블록 및 다른 프로그래밍 언어의 람다와 유사합니다. 라는 정의가 공식문서에 적혀있습니다.  
하지만 이 개념은 이해가 가지 않았고, 개인적으로는 이름이 없는 함수이고 축약을 위한 다른 규칙들이 있다라고 이해하고 있습니다.



### 클로저의 문법
#### 함수의 생김새와 사용방법

```
let name = "hyunho"
func printName() {
  print(name)
}

printName() // hyunho
```

#### 클로져의 생김새
```
let name = "hyunho"
let printName = {
  print(name)
}
printName() // hyunho
```
함수같이 생겨서 구조를 뜯어보았습니다.

```
// 함수

func 함수이름(파라미터) -> 리턴타입 {
    (코드 블럭)
}

// 클로저
{ (파라미터) -> 리턴타입 in
    (코드 블럭)
}
```


### 클로져 축약과정
아래의 예시를 들어 함수를 사용하는 방법과 클로져의 사용을 정리해 보겠습니다.
1. 기본설정  

```
func squared(i: Int) -> Int{ i * i }

squared(i: 5) // 25

let array = [1,2,3,4,5]
```

squared 함수를 만들어 놓고, 5를 입력하면 25를 반환합니다.  
리스트를 만들어서 하나씩 입력 해보겠습니다.


2. 함수와 클로저 사용  

```
// 함수를 매개변수로 전달
let arrayFunctionUsed = array.map(squared)
print(arrayFunctionUsed) // [1, 4, 9, 16, 25]

// 클로저를 매개변수로 전달
let arrayClosureUsedA = array.map({(i: Int) -> Int in return i * i })
print(arrayClosureUsedA) // [1, 4, 9, 16, 25]
```
map을 이용해 인자를 받아 처리한다면 함수를 대입해서 사용할 수 있습니다.  
하지만 한번만 사용할 함수라면 작성하는게 귀찮고 코드만 길어질 뿐이겠죠? 이럴경우 클로져를 대입해줍니다.

3. 타입의 생략  

```
// 이미 배열의 타입이 Int이므로 param도 당연히 Int
let arrayClosureUsedB = array.map({(i) -> Int in return i * i })
print(arrayClosureUsedB)
// [1, 4, 9, 16, 25]
```
array에서 넘어올 타입은 Int 이기 때문에 입력받은 타입은 반드시 Int 입니다. 함수의 정의와 다르게, 
입력받을 타입을 추측할 수 있다는게 신선했습니다.


4. 반환 탑입을 생략  

```
// Int타입을 받아서 연산하기 때문에 결과는 Int이기 때문
let arrayClosureUsedC = array.map({(i) in return i * i })
print(arrayClosureUsedC)
// [1, 4, 9, 16, 25]
```
i의 타입을 알고 있는 상태에서 i*i의 반환 타입을 명시하지 않아도 Int임을 알 수 있습니다. 그래서 생략가능합니다.


5. return 키워드 생략  

```
let arrayClosureUsedD = array.map({(i) in i * i })
print(arrayClosureUsedD)
// [1, 4, 9, 16, 25]
```
클로져가 반환하는 값이 있다면, 마지막 줄의 결과값은 암시적으로 반환값 취급해줍니다. 그러므로 return키워드를 생략할 수 있습니다.  

6. 파라미터 구문 생략

```
let arrayClosureUsedE = array.map({ $0 * $0 })
print(arrayClosureUsedE)
// [1, 4, 9, 16, 25]
```
첫번째 단축인자는 $0으로 표기, 두번째 단축인자는 $1로 표기합니다.

### 중간 결론
함수를 사용할 때 보다 클로저를 쓰면 어떤 장점이 있을까요?
예시와 같이 제곱하는 기능을 한번만 사용한다면, 함수로 구현해 메모리 낭비를 할 필요가 없습니다. 
또한 코드상에서 한번만 쓰이는 코드가 사라지니 가독성에서도 이득을 볼 수 있습니다.


### 후행 클로저
클로저가 함수의 마지막 전달인자라면 마지막 매개변수 이름을 생략한 후에 함수 외부에 클로저를 구현할 수 있습니다.
 
```
let add = { (a: Int, b: Int) -> Int in
    return a + b
}
add(3,4)
```

입력 받은 두 수를 계산해주는 함수를 선언하고, 계산 방법을 인자로 전달받는 함수로 설명 해보겠습니다.
```
func calculate(a: Int, b: Int, method: (Int, Int) -> Int) -> Int {
    return method(a, b)
}
print(calculate(a: 3, b: 4, method: add)) // 7
```

함수 전달 인자 중 마지막 전달 인자가 클로저라면 마지막 매개변수 이름을 생략한 후 함수 소괄호 외부에 중괄호로 구분해 클로저를 구현할 수 있습니다.

```
var result: Int
result = calculate(a: 3, b: 4) { (left: Int, right: Int) -> Int in
    return left + right
}
print(result) // 7
```

### 클로저 함수와 비교  
클로저는 함수와 비교했을 때 뚜렷한 차이점들이 있습니다.

1. 클로저는 이름이 없습니다.
2. 클로저는 매개변수에 라벨값이 없습니다.
3. 클로저는 기본인자 값이 없습니다.
4. 클로저는 인라인의 형태로 작성될 수 있습니다.


### 고차함수
고차함수는 함수의 인자로 다른 함수를 받는 함수를 가리킨다.
map, reduce, filter에 대해 알아봅시다.  

#### map
콜렉션 내부의 데이터를 순회하여 새로운 콜렉션을 생성
```
import Foundation
// --------------------------------------
var prices = [1.50, 10.00, 4.99, 2.30, 8.19]
// --------------------------------------

var arrayForSalePrices: [Double] = []
for price in prices {
  arrayForSalePrices.append(price * 0.9)
}
arrayForSalePrices

let salePrices = prices.map { $0 * 0.9 }
salePrices

let priceLabels = salePrices.map { (price) in
  String(format: "%.2f", price)
}
```
가격이 담긴 리스트가 있을 때, 10% 할인 된 가격이  필요할 경우의 에를 들어보겠습니다.  
for를 사용했을 때 새로운 리스트를 만들고 그 안에 10%할인된 가격을 리스트에 넣어줍니다.  
하지만 map을 사용했을 때 코드가 더 간결해지는 것을 볼 수 있습니다.  

#### reduce  
콜렉션 내부의 데이터를 하나로 통합
```
// --------------------------------------
let ozmaGrades = [60, 96, 87, 42]
// --------------------------------------
let totalGrade = ozmaGrades.reduce(0){ (total, grade) -> Int in
    total + grade
}
totalGrade
```
점수가 담긴 배열의 모든값을 0으로 초기화 된 total 변수에 모두 더해 최종적으로 1개의 값만 남깁니다.
#### filter  
콜렉션 내부의 데이터 중 조건에 맞는 요소들로 이루어진 새로운 콜렉션 생성합니다.
```
// --------------------------------------
let arrayOfDwarfArrays = [
  ["Sleepy", "Grumpy", "Doc", "Bashful", "Sneezy"],
  ["Thorin", "Nori", "Bombur"]
]
// --------------------------------------

let dwarvesAfterM = arrayOfDwarfArrays.flatMap { dwarves -> [String] in
  dwarves.filter { $0 > "M" }
}.sorted()
```
주어진 이름에서 M보다 알파벳 순으로 늦은 이름들만 남겨 새로운 콜렉션을 생성합니다.

### 결론
좀 더 간결한 코딩을 위해 사용하는 클로져를 이해하기 위해서는 원래의 형태가 어떻게 생겼는지 파악하고 어떻게 축약되었는지를 익혀 함수형 프로그래밍의 구조가 눈에 익숙하게 만들어야 합니다.


참고 : [https://nightohl.tistory.com/entry/Swift-%ED%81%B4%EB%A1%9C%EC%A0%80Closure](https://nightohl.tistory.com/entry/Swift-%ED%81%B4%EB%A1%9C%EC%A0%80Closure)




