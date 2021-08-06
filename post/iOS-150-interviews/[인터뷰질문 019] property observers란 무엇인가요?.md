# [인터뷰질문 019] property observers란 무엇인가요?

What are property observers?
Suggested approach: Property observers let you run code before or after a property is modified. Try to give a practical example here: “if you have a score property that holds an integer, you might attach a didSet observer so that it updates a label whenever the score changes.”

For bonus points mention the major problem they have: you might think that adding 1 to an integer is a nice and quick operation, but if you accidentally attach a complex property observer then it could cause havoc.
