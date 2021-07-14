# [인터뷰질문 009] one-sided ranges가 무엇이며 언제 사용할 수 있나요?

## 범위 연산자
범위를 나타내는 여러가지 방법이 있습니다.
- closed range operator
  - 0...5 // [0, 1, 2, 3, 4, 5]
- half-open range operator
  - 0..<5 // [0, 1, 2, 3, 4]
- one-sided range operator
  - ...5 // ??

특정 조건에서는 `one-sided ranges`를 사용할 수 있습니다. 에를 들면 아래의 코드 같은 경우 입니다.

```
let friends = ["Leeo", "Jonh", "Tim", "Lisa", "Mons"]
let member = friends[..<3] // "Leeo", "Jonh", "Tim"
```

## 활용
배열을 잘라서 사용해야 할 떄, 일부분을 자르는게 아닌, 처음부터 어디까지 혹은 어디부터 끝까지 자를 떄 활용하면 편리합니다.

어디서 부터 시작인지, 어디까지가 끝인지 명시하지 않아도 잘리기 때문이죠.
