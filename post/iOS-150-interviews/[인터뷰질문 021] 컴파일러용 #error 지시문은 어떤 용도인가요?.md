# [인터뷰질문 021] 컴파일러용 #error 지시문은 어떤 용도인가요?

What does the #error compiler directive do?

Suggested approach: Answering this isn’t hard (“it forces the compiler to emit an error using a message we specify”), but please make sure you follow up with at least one example. So, maybe you’re shipping some sample code where users need to enter an API key otherwise it won’t work, so you use #error next to the API key line saying “fill in your API key before continuing.” Similarly, you might use #error alongside an OS check to say that your code isn’t compatible with tvOS, for example.
