# [인터뷰질문 023] assert() 함수는 무엇을 하나요?

관련주제 : Swift
난이도 : 하

## 디버깅
디버깅 모드에서 내 코드가 정상동작하는지 확인하기 위한 함수 입니다. 예를 들면, `average = total / count` 가 있을 때 `count`가 0이 될 수 있다면 문제가 발생합니다. 그 상황을 디버깅 하기 위해서 물론 출력을 해도 됩니다.

if count == 0 { print("0이 되었습니다") }

하지만 너무나 불편하기 때문에, `assert(count != 0, "0이 되었습니다")` 로 좀 더 의도를 명확히 할 수 있습니다.

assert 함수를 어떻게 쓸 수 있는지는 [공식 문서](https://developer.apple.com/documentation/swift/1541112-assert)를 참고하는 편이 명확합니다.

간단히 설명드리면 `assert(조건, "조건문이 false 일 때 출력할 메시지")` 입니다.
