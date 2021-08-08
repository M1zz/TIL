# [인터뷰질문 020] raw strings 이란 무엇인가요?

Suggested approach: There’s a simple formula here, taking you from broad to specific: explain how you make raw strings, explain how you’d use them, then provide any extra information such as how they handle string interpolation.

So, you might start by saying that you make them by placing a hash before and after your string quotes, they are used for strings where lots of instances of quote marks or backslashes appear in order to make the string easier to read (bonus points for mentioning regular expressions!), and then finish up by saying that string interpolation must also use a hash in order to become active.

If you want to really show off your knowledge, perhaps mention that you can use more than one hash if you need to.
