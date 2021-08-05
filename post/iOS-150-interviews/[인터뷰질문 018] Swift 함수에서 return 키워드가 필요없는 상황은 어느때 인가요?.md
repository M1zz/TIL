# [인터뷰질문 018] Swift 함수에서 return 키워드가 필요없는 상황은 어느때 인가요?



when it is supposed to return a value but you’ve used something like fatalError() to skip that requirement,


when it returns a value using a single expression.

That second case is useful when you have placeholder functions you haven’t implemented yet, or

have created an abstract class where child classes will override your erroring implementations.
