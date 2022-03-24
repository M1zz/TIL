# [인터뷰질문 039] What does the #available syntax do?

관련주제 : Swift
난이도 : 중

Suggested approach: This syntax was introduced in Swift 2.0 to allow run-time version checking of features by OS version number. It allows you to target an older version of iOS while selectively limiting features available only in newer iOS versions, all carefully checked by the compiler to avoid human error.
