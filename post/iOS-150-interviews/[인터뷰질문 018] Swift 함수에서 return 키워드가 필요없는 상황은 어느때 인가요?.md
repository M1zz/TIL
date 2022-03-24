# [인터뷰질문 018] Swift 함수에서 return 키워드가 필요없는 상황은 어느때 인가요?

관련주제 : Swift
난이도 : 하

## return 키워드
`return` 키워드는 함수의 반환값을 표시할 때 쓰입니다. 이 함수에서 이 값이 반환됩니다 라는 의미지요.
함수가 무엇인지 반환값이 무엇인지 모른다면 먼저 공부하고 읽으시는 것을 추천 드립니다.

## return값이 없을 때
함수에 반환값이 필요 없는경우 당연히 return 키워드가 필요하지 않습니다.

```
func sayHello() -> Void {
    print("Hello")
}
sayHello() // Hello
```

## Error 상황
함수가 실행되다가 정상적인 종료가 아닌 예외처리 하는 부분에서는 return 키워드가 필요 없습니다.

```
func devidedValue(numerator : Double, denominator: Double) -> Double {
    if denominator == 0 {
        fatalError("can not devided by zero")
        //print("can not devided by zero")
    } else {
        return numerator / denominator
    }
}

print(devidedValue(numerator: 3, denominator: 6))
print(devidedValue(numerator: 3, denominator: 0))
```
함수에서 반환타입이 있다면 반환값을 지정해주어야 합니다. 하지만 `fatalError()` 구문이 있으면, return 하지 않아도 됩니다. 왜냐하면 `fatalError()`을 만나는 순간 함수가 종료되기 때문입니다.

## 한줄로 작성된 함수의 반
```
func devidedValue(numerator : Double, denominator: Double) -> Double {
    numerator / denominator
}

print(devidedValue(numerator: 3, denominator: 6))
```
위와 같이, 반환타입이 존재하는데, 한줄만 작성되어있다면, 해당 값 앞에 붙어야 할 return 키워드가 없어도 반환해줍니다. 이 케이스는, 정말 return 키워드가 없어도 될 곳에 작성하는 것이 좋습니다. 아무리 간결하더라도 생략한다면, 코드를 읽는 사람이 힘들 수 있기 때문이죠.
