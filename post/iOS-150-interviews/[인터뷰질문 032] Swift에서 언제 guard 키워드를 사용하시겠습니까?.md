# [인터뷰질문 032] Swift에서 언제 guard 키워드를 사용하시겠습니까?

관련주제 : Swift
난이도 : 하

Suggested approach: It’s most commonly used to check preconditions are satisfied, but you should also discuss how variables it creates remain in scope after the guard block, and also how it enforces you exit the scope if the precondition fails.

For bonus points, mention that you can use guard inside any kind of block as long as you escape afterwards – you can use it inside a loop for example.
