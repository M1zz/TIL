# [인터뷰질문 024] 컴파일러 조건인 canImport()은 어떤 역할을 하나요?

관련주제 : Swift
난이도 : 하

## 활용방법
`#if문`의 state문 중 하나입니다. canImport() 안에 는 모듈이름이 들어가고, import 할 수 있으면 true를 반환합니다.

특정 코드에서 사용하는 모듈들이 조금씩 다를 때 분기를 해줍니다.
예를 들면 iOS에서는 UIKit을 사용하고, macoS에서는 AppKit을 사용한다면 분기를 해주어야겠죠?

사용에제는 아래와 같습니다.
```
#if canImport(UIKit)
  import UIKit
#endif

#if canImport(AppKit)
  import AppKit
#endif
```
