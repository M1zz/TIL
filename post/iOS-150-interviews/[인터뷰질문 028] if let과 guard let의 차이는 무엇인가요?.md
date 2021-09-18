# [인터뷰질문 028] if let과 guard let의 차이는 무엇인가요?


Suggested approach: Both check and unwrap optionals, but guard forces an early return if its check fails – your code will literally not compile unless you exit the scope. Furthermore, any variables that guard unwraps stay in scope after the guard block, whereas with if let the variables are available only inside the scope.
