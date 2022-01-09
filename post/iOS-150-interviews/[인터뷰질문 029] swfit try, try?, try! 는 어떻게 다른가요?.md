# [인터뷰질문 029] swfit try, try?, try! 는 어떻게 다른가요?

기본적으로 `try`는 예외처리를 할 때 사용합니다.

Suggested approach: Think about the underlying goal here: why would you want to say “you cannot inherit from this class”? Is that a good idea? There are good reasons for using it – sometimes a class does something very precise, and you really don’t want users to override important parts of your code.
