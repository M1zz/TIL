### 오류처리
프로그래밍을 하다보면, 모든 예외처리를 해 줄 수 없기 때문에, 미리 예상이 되는 오류들을 정의 해 놓고
발생되면 개발자에게 알려주는 방법으로 위기를 줄이는 방법이 있습니다. 디버깅을 하면서 어디서 부터 문제가 시작되는지 알려주는 게 있었으면 좋겠다라고 생각했는데, 오류 처리를 해 놓으면 문제 추적에 실마리가 될 것입니다. 한 번도 직접 해보지 못했던 오류 처리를 정리해 보도록 하겠습니다.

### 오류를 정의하기
오류를 정의하는 방법은 아주 간단합니다. enum으로 오류의 케이스들을 정의 해 놓고 Error프로토콜을 채택 해 주면 됩니다.

날짜(YYYY-MM-DD)를 파싱하는 메소드에서 발생할 수 있는 에러에 대해 정의 해 보겠습니다.  
1. 입력받은 값이 날짜 형식보다 길다
2. 입력받은 값이 날짜 형식보다 짧다
3. 숫자가 아닌 형식이다
4. 입력값의 범위가 날짜의 값보다 크거나 같다(13월 32일)

```
enum DatePareseError: Error {
    case overSizeString
    case underSizeString
    case incorrectFormat(part: String)
    case incorrectDate(part: String)
}
```
### 오류 던져주기
오류는 던져줍니다. 이게 무슨 뜻이나면 `func pareDate(:NSString) -> Date` 날짜를 파싱하는 메소드가 있을 때 오류가 발생되면, 에러를 반환? 해야합니다. 하지만 우리는 이미 정상동작일 때 문자열을 반환한다고 정의 해 놓았으니 에러는 반환되는 것이 아니라 던져줘야 합니다. 

그러면 어떻게 던질 수 있을까요? 바로 반환값 앞에다가 `throw`를 써줍니다. `func pareDate(:String) throws -> String`

```
func parseDate(param: NSString) throws -> Date {
    guard param.length == 10 else {
        if param.length > 10 {
            throw DatePareseError.overSizeString
        } else {
            throw DatePareseError.underSizeString
        }
        
    }
    ...
}
```
이렇게 작성된 메소드가 있습니다. 입력받은 문자열의 길이가 10이 아니라면, YYYY-MM-DD의 형태가 아니라는 뜻 이기 때문에 입력값의 오류 입니다. 이 때 우리는 오류를 던져(throw)줍니다.

### 오류 줍기
오류는 던져지기 때문에 주워야 합니다. :) 그럼 어떻게 줍는지 살펴 보도록 하겠습니다. 만약에 에러가 발생하면 던지는 것이기 때문에, 당연히 일반적으로는 정상동작을 할 것 이라고 기대합니다.

```
do {
    let result = try parseDate(param: "20200131")
} catch  DatePareseError.overSizeString {
    print("문자열이 10보다 깁니다.")
} catch  DatePareseError.underSizeString {
    print("문자열이 10보다 짧아요.") // 
}

//문자열이 10보다 짧아요.
```

### 정리
잘 작성된코드, 좋은 개발자는 완벽하게 코드를 짤 수 있는 사람보다도, 에러를 찾기 쉽게 코드를 작성해 해준 개발자, 에러를 찾기 쉬운 코드라고 생각합니다. 앞으로 코드를 짤 때, 어떻게 하면 오류를 처리할 수 있는지에 대해 더 공부해서 남들과 같이 쓸 쑤 있는 코드를 짤 수 있도록 해야겠습니다.











