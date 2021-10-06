# [인터뷰질문 028] if let과 guard let의 차이는 무엇인가요?

if let과 guard let은 비슷하면서도 느낌이 다릅니다.
```
if let code = optional?.code {
    // 값이 있으면
} else {
    // 값이 없으면
}

guard let code = optional?.code else {
    // 값이 없으면
}

// 값이 있으면
```

if let은 옵셔널 값이 있으면 동작하는 코드와, 없으면 동작하는 코드의 형태로 되어있습니다.

하지만 guard let은 옵셔널 값이 있어야만 동작하도록 코드를 짤 수 있기 때문에 좀 더 필요성을 강조할 수 있습니다. 또한 옵셔널한 값이 비어있으면 함수가 조기에 종료됩니다.

언래핑한 옵셔널 값의 사용가능한 범위가 `if let`은 구문 안이지만 `guard let`은 구문 밖에서도 사용 가능합니다.
