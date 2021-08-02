# [인터뷰질문 016] operator overloading을 주니어 개발자에게 어떻게 설명해 줄 것 인가요?

## 오버라이딩
이 문제를 이해하기 위해서는 오버라이딩이라는 개념을 알고 있어야 합니다. 오버라이딩은 쉽게 말해 같은 이름의 다른 타입의 함수를 정의하는 것 입니다. 두개의 인자를 받아서 더하는 함수를 정의해 보겠습니다.

```
func sum(_ a: Int, _ b: Int) -> Int {
    return a + b
}

func sum(_ a: Double, _ b: Double) -> Double {
    return a + b
}

print(sum(3, 2)) // 5
print(sum(4.1, 4.2)) // 8.3
```

두 숫자의 합을 구하고 싶을 때, 매개별수로 정수와 실수가 들어가야 한다면, 이렇게 한 함수를 오버라이딩해서 사용하는 사람이 편하게 쓸 수 있도록 구현할 수 있습니다.

## 연산자 오버라이딩
이름에서 알 수 있듯이, 연산자를 오버라이딩 하는 것 입니다. 

it allows us to use the same + operator with multiple types, such as integers, strings, doubles, and more. If you want to talk about examples of custom operator overloading, perhaps think about multiplying a CGPoint or something else that’s easy to use in practice.
