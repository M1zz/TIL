# [인터뷰질문 002] Swfit에서 class와 struct의 차이점이 무엇인가요?

https://www.hackingwithswift.com/interview-questions/what-are-the-main-differences-between-classes-and-structs-in-swift
## 값 타입, 참조 타입
많은 분들이 머리로는 다 알고 있는 부분일것이라 생각합니다. 저도 `class`는 reference type 이고,
`struct`는 value types이다. 라고 알고 있습니다.

그게 무슨 뜻일까요? 쉽게 비유하면, 엑셀과 스프레드 시트로 할 수 있습니다.
![엑셀스프레드](https://cdn.wallstreetmojo.com/wp-content/uploads/2018/12/Excel-vs-Google-Sheets.jpg)

여러분이 하나의 엑셀을 만들고 다른 동료에게 보내고 난 다음에, 그 엑셀이 어떻게 되었는지는 관심도 없고 나에게 영향을 주지 않죠. 하지만 내가 스프레드 시트를 만들어서 전해주면, 다른사람이 내가 만든 결과물에 영향을 줄 수 있습니다.

너무 동떨어진 예제라면 아래 코드로 이해해 보도록 하겠습니다.

```
struct Student {
    var name: String
    var age: Int
}
```
간단한 예제의 `struct`가 있습니다. `name`과 `age`를 가지고 있습니다.


```
let firstStudent = Student(name: "Leeo_student", age: 17)
print(firstStudent.name, firstStudent.age) // Leeo_student 17

var secondStudent = firstStudent
secondStudent.name = "batgird_student"
secondStudent.age = 18

print(firstStudent.name, firstStudent.age) // Leeo_student 17
print(secondStudent.name, secondStudent.age) // batgird_student 18

```
처음에 생성하고 바로 출력해보면, 생성한 결과가 나오니 이상하진 않습니다. 그리고 두번째 변수를 만들어서 대입해주고, 값을 할당 해줍니다.
그리고 다시 두 변수 다 출력 해보면, 우리가 입력한 결과가 출력됩니다. 마치 내가 작업하던 엑셀을 복사해서 하나 더 만들어 작업한 것과 같은 상황이죠.

그렇다면 class는 어떨까요?

```
class Friend {
    var name: String
    var age: Int

    init(name: String, age: Int) {
        self.name = name
        self.age = age
    }
}
```
위에서 봤던 struct와 같은 class입니다.

```
let firstFriend = Friend(name: "Leeo", age: 27)
print(firstFriend.name, firstFriend.age) // Leeo 27

var secondFriend = firstFriend
secondFriend.name = "batgird"
secondFriend.age = 28

print(firstFriend.name, firstFriend.age) // batgird 28
print(secondFriend.name, secondFriend.age) // batgird 28
```
내가 바꾼건 `secondFriend`이지만, `firstFriend`과 같은 인스턴스를 바라보고 있기 때문에 영향을 받고 있습니다.

여담이지만 `struct`와 다르게 `class`는 `reference type` 이기에 메모리 관리가 필요합니다. 그래서 다 사용하고나면, 메모리의 사용을 줄이기 위해 deinit이 호출됩니다. 다음 예제는 사용 후, deinit까지 호출된 모습입니다.

```
var thirdFriend: Friend? = Friend(name: "mizz", age: 32)
print(thirdFriend!.name, thirdFriend!.age) // mizz 32
thirdFriend = nil // good bye my friend
```


이제 차이가 느껴지시나요? 그럼 여기서드는 질문이 하나 있습니다.
언제 struct, class를 골라써야하나요?

## Value Type을 써야하는 순간
인스턴스의 데이터를 == 와 같은 비교를 해야하는 순간에는 밸류 타입을 써야합니다.

```
struct Point: CustomStringConvertible {
  var x: Float
  var y: Float

  var description: String {
    return "{x: \(x), y: \(y)}"
  }
}
```

```
let point1 = Point(x: 2, y: 3)
let point2 = Point(x: 2, y: 3)
```
위의 예제를 살펴보겠습니다. `point1`과 `point2`가 같은 좌표계이지만 다른 두 위치를 나타낸다는 것을 알고 있습니다.

같은 형태로 다른 인스턴스를 만들어 내야한다면, `struct`를 쓰면 좋습니다.

`multiple threads`에서 동작하게 된다면 `Value Type`을 사용하는 것이 좋습니다. 왜냐하면, 멀티플 쓰레드에서는 언제 어떻게, 값이 변할지 모르기 때문에 여러군데에서 값을 변경하면 원하지 않는 버그가 발생할 수 있기 때문이죠. 만약 여러 쓰레드에서 값을 변경하고 싶다면, 다른 방법을 사용해야합니다.

## Reference Type을 써야하는 순간
`===`로 비교해야하는 순간에는 `Reference Type`을 써야합니다. `===` 연산자는 메모리 상의 인스턴스가 같음을 비교하기 때문에, 참조형이 필요하겠죠?

생성한 인스턴스가, 공유되고 상태가 계속 변경되어야 한다면 `Reference Type`을 써야합니다.

일반적으로 많이 사용되는 예를 이용하여 나타내 보겠습니다.

```
class Account {
    var balance = 0.0
}

class Person {
    let account: Account

    init(_ account: Account) {
        self.account = account
    }
}
```
위와 같은 클래스가 있을 때, class로 짰다는 뜻은 사람의 잔액을 이 곳 저곳에서 접근해서 변경할 것이고, 항상 모두가 같은 잔액을 바라보고 있을 수 있다는 장점이 있습니다.
```
let account = Account()

let person1 = Person(account)
let person2 = Person(account)

person2.account.balance += 100.0

person1.account.balance    // 100
person2.account.balance    // 100
```
