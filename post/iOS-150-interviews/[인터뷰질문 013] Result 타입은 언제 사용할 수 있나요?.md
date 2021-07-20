# [인터뷰질문 013] Result 타입은 언제 사용할 수 있나요?

Suggested approach: Start with a brief introduction to what Result does, saying that it’s an enum encapsulating success and failure, both with associated values so you can attach extra information. I would then dive into the “when would you use it” part of the question – talking about asynchronous code is your best bet, particularly in comparison to how things like URLSession would often pass both a value and an error even when only one should exist at a time.

If you’d like to go into more detail, more benefits of Result include being able to send the result of a function around as value to be handled at a later date, and also the ability to handle typed errors.
