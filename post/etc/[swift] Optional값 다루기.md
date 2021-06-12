변수의 타입 뒤에 `?`이 붙어있는 데이터를 사용하다가 자꾸 에러가 떠서 검색해보니 Optional이다. Optional에 대해 이번기회에 정리하려 한다.

### Optional의 구현
Optional은 enum으로 구현되어있다.

```
enum Optional<T> { 
	case none 
	case some(<T>) 
}  
```
optional의 타입은 generic이다. 그러므로 Int, String 등 모든 타입이 들어갈 수 있다.
Optional에는 두 종류가 있는데, 하나는 none이고, 나머지 하나는 some(<타입>) 이다. none은 아직 타입이 설정되지 않은 경우이고, some은 선언 된 타입의 값을 가지고 있다.

Optional은 enum으로 구현되어 있지만 enum으로 사용하지 않는 이유는 Optional을 사용할 때마다 switch문을 사용하는 방법이 번거롭기 때문이다. switch문을 사용하면 항상 같은 none과 some에 대한 경우를 구현해야하니 코드가 길어질 것이다. 그래서 ?, !, ?? 등으로 Optional을 사용할 수 있게 만든것이다.

아래 예를 들어 Optional의 특징에 대해 설명한다. 거주민의 차 현황에 대해 입력하는 아래와 같은 코드가 있다.

```
var residentName: String = "John"
var age: Int = "23"
var carNumber: String = "1422"
```

residentName과 age는 모든 사람이 가지고 있지만 carNumber는 없을 수도 있다.

빈칸을 입려할 수 있지만. 값이 없는 상태를 나타내는 nil값을 넣는 방법도 있다.

### Optional 사용방법

변수의 타입 뒤에 ? 를 붙여준다. 그러면 해당 타입의 데이터를 입력할 수 있고, 아무값도 아닌 nil값을 입력할 수 있다.

```
var carNumber: String?
carNumber = "1422"

carNumber = nil
```



위의 코드를 출력하면 다음과 같은 결과가 나온다.
```
print(carNumber)
// Optional("1422")

print(carNumber)
// nil
```

변수로 사용하면 어떤 결과가 있을까?
```
print(carNumber+"1")

error: value of optional type 'String?' must be unwrapped to a value of type 'String
```

optional변수의 값을 가져오는데는 2가지 방법이 있다. 

1.  FORCE UNWRAPPING
2.  IF LET BINDING

#### FORCE UNWRAPPING

변수 뒤에 !를 붙여 강제로 랩핑을 벗기는 방법이다.

하지만 nil값이 들어있는 Optional Type을 벗기려 한다면, 프로그램이 죽어버린다.

```
var carNumber: String?
carNumber = "1422"
print(carNumber!+"1")

// 14221

var carNumber: String?
carNumber = nil

print(carNumber!+"1")
// Terminated by signal 4
```

위와 같은 이유로 조심해야 하기 때문에 좀 더 안전한 방법을 사용한다. 조건문을 이용해 사용할 수 있다.
```
var carNumber: String?
carNumber = nil

if carNumber != nil {
  print("car number is \(carNumber!)")
}
else {
  print("car number is \(carNumber)")
}
```

#### IF LET BINDING
조금 더 명확하게 변수를 선언해서 사용할 수 있다.

```
var carNumber: String?
carNumber = nil

if let unwrappedCarNumber = carNumber {
  print("car number is \(unwrappedCarNumber)")
}
else {
  print("car number is \(carNumber)")
}
```

`!`를 사용한 force unwrapping 방식보다 안전한 방법이다.

#### COALESCING (nil 병합연산자)

옵셔널 값을 안전하게 랩핑을 벗기는 방법은 위에서 소개했듯이 아래와 같다.

```
var optionalCarNumber: String?
var carNumber: String

if optionalCarNumber != nil {
    carNumber = optionalCarNumber!
}
```

하지만 코드가 하는 일에 비해 길어지고, 보다 간결하게 작성할 수 있다.

```
var optionalCarNumber: String?
optionalCarNumber = nil
var carNumber = (optionalCarNumber != nil ? optionalCarNumber! : "empty")

print(carNumber)
// empty
 ```

삼항 연산자는 코드의 가독성이 떨어진다. 이때 사용하는 것이 nil 병합연산자 이다.

`??`기호를 사용한다.

```
var optionalCarNumber: String?
optionalCarNumber = nil

var carNumber = optionalCarNumber ?? "empty"

print(carNumber)
// empty
```

### summary

Optional 값은 값이 없을 때 nil을 입력받을 수 있게 해준다. 하지만 nil값이 들어있을 때 잘 못 접근하면,

프로그램이 죽는 위험 이기도 한다. 이를 안전하게 사용하기 위한 방법이 필요하다.

- if let
- coalescing
