같은 코드를 복사 붙여넣기 하면서 쓰지 않기 위해 함수를 사용한다.
함수의 기능에 대해 정리하면서 남긴 기록.

### 정의하는 방법
사용법은 간단하다 func 키워드 뒤에 함수 이름과 ()를 붙여주면된다.
```
func printHello(){
  print("hello")
}
```

### 사용하는 방법
간단하다 함수이름과 빈()를 붙여주면 된다.
```
printHello()
// hello 출력
```

### 인자 넘겨주기
()사이에 이름과 타입을 명시해주면 된다.
```
let speedLimit = 100
let mySpeed = 80

func printSpeedViolationStatus(speed: Int){
  print(speed > speedLimit ? 'Speed violation!' : 'Keep driving.')
}

printSpeedViolationStatus(speed: mySpeed)
// Keep driving.
```

여러개의 인자를 넘겨주는 방법은 , 로 추가해서 넘겨준다
```
let speedLimit = 80
let mySpeed = 100

func printSpeedViolationStatus(speed: Int, speedLimit: Int){
  print(speed > speedLimit ? 'Speed violation!' : 'Keep driving.')
}

printSpeedViolationStatus(speed: mySpeed, speedLimit: speedLimit)
// Speed violation!
```

### 인자 기본 값(Default Parameter Values)
기본값을 넣을 수 있다.  
speedLimit: Int = lowLimit 와 같이 작성한다.
```
let mySpeed = 100
let lowLimit = 80

func printSpeedViolationStatus(speed: Int, speedLimit: Int = lowLimit){
  print(speed > speedLimit ? 'Speed violation!' : 'Keep driving.')
}

printSpeedViolationStatus(speed: mySpeed)
// Speed violation!
```

### 외부 호출 인자 (External Parameter Names)
함수에서 인자를 호출 할 때 외부에서 호출하는 이름을 다르게 줄 수 있다. 사용에 따라 용이한 경우에 이용한다.  
for speed: Int 의경우 함수가 호출될 때 speed 변수가 값을 넣으려면 for를 이용해 넣어주어야 한다.   
```
let benz = 130
let audi = 79
let lowLimit = 80

func printSpeedViolationStatus(for speed: Int, speedLimit: Int = lowLimit){
  print(speed > speedLimit ? 'Speed violation!' : 'Keep driving.')
}
printSpeedViolationStatus(for: benz)
// Speed violation!
```

### 이름없는 인자
함수를 선언 할 때 인자의 이름에 의미가 없을 경우 이름을 사용하지 않을 수 있다.
인자의 이름이 구별하는데 인자의 뜻을 구별하는데 의미가 없다면 사용하지 않는 것이 코드를 이해하는데 도움이 될 수 있다.
```
//  func printFastestSpeed(speed1: Int, speed2: Int){
//    print(speed1 >= speed2 ? speed1 : speed2)
//  }
//  printFastestSpeed(speed1: 80, speed2: 100)

func printFastestSpeed(_ speed1: Int, _ speed2: Int){
  print(speed1 >= speed2 ? speed1 : speed2)
}
printFastestSpeed(80,100)
// 100
```

### 반환값 (Returning Value)
반환값이 있는 함수는 호출된 자리에 반환값을 반환한다.  
```
let speedLimit = 80
let mySpeed = 100
let lowLimit = 80

func printSpeedViolationStatus(speed: Int, speedLimit: Int = lowLimit) -> Bool{
  return speed > speedLimit
}
printSpeedViolationStatus(speed: mySpeed)
// true
```
한줄로 작성된 값을 반환하는 함수의 경우 return 키워드를 생략할 수 있다.
```
func printSpeedViolationStatus(speed: Int, speedLimit: Int = lowLimit) -> Bool{
  speed > speedLimit
}
```

### 함수의 이름 짓기(Naming Function)
함수가 어떤 행동을 해야하는지 동사로 시작하고, 값에대한 명사룰 이용햐 지어준다.
```
func printSomething(){
  print("something")
}

func calculateAmountOff() -> Int{
  abs(targetAmount - myAmount)
}
```
