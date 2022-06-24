[003 Coding tip of the day - switch vs if]
조건의 패턴이 여러개라면 if문 대신에 switch문을 써보세요.
if문에는 어느 조건이든 들어올 수 있고, 그 케이스가 몇 개인지 알기 어렵습니다. 하지만 switch는 enum과 함께하면 해당 케이스만 따로 고민할 수 있습니다. 제약이 명확해집니다.

![003](./images/003.png)
