# [인터뷰질문 020] raw strings 이란 무엇인가요?

## raw strings
`raw strings`는 swift5에 추가된 자연스러운 String을 쓰는데 도움을 주는 기능입니다. 지연스러운 String은 어떤 걸까요? 아래의 문자열을 비교해 보겠습니다!

```
let naturalString = #""Hello world!""#
let unNaturalString = "\"Hello world!\""

print(naturalString) // "Hello world!"
print(unNaturalString) // "Hello world!"
```

익숙함에 따라 어떤 문자열이 더 자연스러운지가 다를 수 있을텐데요, 설명을 드리자면 `#`기호 사이에 `""` 이렇게 문자열을 나타내주는 기호가 있다면, 그 사이에 있는 어떤 특수 문자도 자연스럽게 처리해줍니다.  
기존에는 `\`기호를 특수 문자 앞에 붙여서 이 기호가 특수 기호인지 나타낼 필요가 없습니다. 말 그대로 자연스러운 문자열을 만들 수 있는 것입니다.

만약 문장 안에 `#`기호를 사용해야 한다면, `##`기호를 문장의 시작과 끝에 붙여주면 됩니다.

## 활용법
기존에 문자열 사이에 변수를 넣는 방법과 다르니 유의 해주세요!
```
let name = "Leeo"

let normalString = "Hello, \(name)!"
let rawString = #"Hello, \#(name)!"#
let wrongRawString = #"Hello, \(name)!"#

print(normalString) // Hello, Leeo!
print(rawString) // Hello, Leeo!
print(wrongRawString) // Hello, \(name)!
```

## 메리트
특정 상황에서 사용했을 때, 메리트를 보입니다.
예를 들면 `\`기호를 사용하는 정규식에서의 사용입니다.

`"https://apple.com"` 이라는 문자열의 정규식은  `\\[A-Z]+[A-Za-z]+\.[a-z]+`입니다. 하지만 실제로 코드로 옮겨 보면 다음과 같습니다
`\` 기호를 인식하기 위해서 매우 복잡해졌습니다.
```
let regex = try NSRegularExpression(pattern: "\\\\[A-Z]+[A-Za-z]+\\.[a-z]+")
```
rawString을 쓰면 다음과 같이 바뀝니다.
```
let regex = try NSRegularExpression(pattern: #"\\[A-Z]+[A-Za-z]+\.[a-z]+"#)
```
원래의 정규식을 그대로 사용할 수 있으니, 매우 가독성이 좋아지죠?
잘 몰랐던 새로운 `raw strings`에 대해 알아보았습니다.
