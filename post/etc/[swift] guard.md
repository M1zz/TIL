처음 가드문을 접한것은 옵셔널 체인의 guard let 에서 였습니다. 그래서인지, 오히려 if let 과 guard let 이 비슷 해 보였고 헷갈리기 때문에 정리 해 두려고 합니다.

if문은 너무나 익숙 하기 때문에 따로 정리하지 않겠습니다. 

### guard
형태는 다음과 같습니다. 
```
guard 조건 else {
  조건이 false
  return || throw
}
```
뭔가 if문과 비교한다기 보다는 생긴 것 그대로 해석을 해 보면, 특정 조건으로 부터 guard 해줍니다.
조건이 틀린 경우는 모두 버리고, 우리가 원하는 조건만 통과 시키겠다는 의미 입니다. 마치 성문 앞에서 통행증을 보고 통과 시켜주는 가드가 생각나는 군요.

### 사용 예제
guard문은 조건이 맞지 않으면 종료 시켜야 하도록 문법이 되어있기 때문에, 반드시 return이나 throw를 해주어야 합니다. 반대로 말하면 함수 내부에서 사용하도록 만들어진 녀석이죠.

함수에서 사용할 조건들에 맞지 않으면 걸러 보겠습니다. 

```
func guardTest() {
    let condition1: Bool = true
    let condition2: Bool = true
    guard condition1,condition2 else {
        return print("Bye!")
    }
    print("Come In!")
}
guardTest()
```
조건 2개를 검사하고 모두 만족할 경우에 들여보내드록 하겠습니다. 만약 조건 중 하나라도 거짓이라면 그 들은 내쫓길 것 입니다.

### 느낀 장점
개인적으로 느낀 장점은 코드의 가독성이 좋아집니다. 예를 들면 우리가 어떤 함수에서 실행해야 하는 조건이 3개라면, if문으로 썼을 때는 다음과 같습니다.
```
func solution() {
    if condition1 {
        if condition2 {
            if condition3 {
                print("Come In")
            }
            else {
                print("Bye!")
            }
        } else {
            print("Bye!")
        }
    } else {
        print("Bye!")
    }
}
```
물론 조건문을 좀 더 이쁘게 쓰면 가독성이 좋아질 수 있습니다. 다음은 가드를 사용했을 때 입니다.
```
func solution() {
    guard condition1 else { return print("Bye!") }
    guard condition2 else { return print("Bye!") }
    guard condition3 else { return print("Bye!") }

    print("Come In")
}
```
이 함수가 무엇을 하고 싶은지 좀 더 명확하게 보입니다.

### 정리
함수의 실행 조건을 좀 더 가독성 좋게, 간결하게 작성하고 싶을 때 guard를 한번 써보시는건 어떠신가요?

