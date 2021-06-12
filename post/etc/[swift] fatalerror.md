코드를 리팩토링 하다 에러문구를 만났고, Fix를 누르니 다음과 같은 코드가 생겼다. 이미 init 함수는 있는데 무슨 용도일까 해서 찾아 보았다.

```
required init?(coder aDecoder: NSCoder) {
    fatalError("init(coder:) has not been implemented")
}
```

`override init(frame: CGRect)`은 view를 만들 때 사용된다.  
`required init?(coder: NSCoder)` 스토리 보드에 생성될 때 사용된다.

fatalError는 언제 무슨 용도로 쓰는지 알아보자.

[설명](https://developer.apple.com/documentation/swift/1538698-fatalerror)은 다음과 같았다. `Unconditionally prints a given message and stops execution.` 무조건 출력하고 실행을 멈춘다?

정의는 다음과 같다.
```
func fatalError(_ message: @autoclosure () -> String = String(), file: StaticString = #file, line: UInt = #line) -> Never
```
Never을 반환한다. 이 의미는 호출이 되면 더 이상 앱이 진행되지 않는다. 즉 개발자가 인지 하지 못하는 상황에서 예외처리를 이 함수로 한다면, 실행 되었을 때 문제가 있는 부분을 알 수 있다.

다른 예를 하나 더 들어보자. 
테이블 뷰를 쓸 때 자주 구현하는 메소드이다.
```
override func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell {
    guard let cell = tableView.dequeueReusableCell(withIdentifier: "Cell", for: indexPath) as? MyCustomCell else {
        fatalError("Failed to load a MyCustomCell from the table.")
    }

    return cell
}
```
다음과 같은 코드가 실행될 때, cellForRowAt 메소드는 UITableViewCell을 리턴해야하지만 재사용 가능한 셀을 큐에서 빼고 사용자 정의 셀 유형으로 조건부로 타입캐스트 하려고하면 어떻게 되겠는가?

보통 구성되지 않은 빈 셀을 반환하려고 시도 할 수 있다. 하지만 실제로는 별로 의미가 없다. 잘못된 셀을 다시 가져 오면 버그가 발생할 수 있기 때문에 이런 경우에는 강제로 에러를 만들어 주어야 한다.
