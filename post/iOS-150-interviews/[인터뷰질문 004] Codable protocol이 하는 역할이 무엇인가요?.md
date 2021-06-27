# [인터뷰질문 004] Codable protocol이 하는 역할이 무엇인가요?

## Cadable이란?
encodable과 decodable 프로토콜을 모두 채택한 프로토콜 입니다. 데이터를 손쉽게 Json이나 다른 구조화된 데이터로 인코딩 하거나, 파싱할 때 사용합니다.

## 예제
```
import Foundation

struct Student: Codable {
    var name: String
    var age: Int
}

let studentData = """
{
    "name": "leeo",
    "age": 17
}
""".data(using: .utf8)!

let student1 = try! JSONDecoder().decode(Student.self, from: studentData)
print(student1) // Student(name: "leeo", age: 17)
print(student1.name) // leeo
print(student1.age) // 17
```

모델을 만들고, `Codable` 프로토콜을 채택하면 `Decodable` 프로토콜이 포함되어 있기 때문에  `JSONDecoder().decode()`를 사용할 수 있습니다.

그러면 `from: studentData`이 부분에 입력된 데이터와 `Student.self`이 같은 타입인지 비교하고 일치하면 `Student`의 객체로 만들어 줍니다.

## 사용할 때의 문제점1
swift 에서 사용하고 있는 변수는 `camelCase`를 사용하고 있습니다. 하지만 내려오는 데이터도 같은 키로 내려오리란 보장이 없죠 그 때 사용할 수 있는것이 `CodingKey`입니다. 같은 키는 알아서 찾지만, 다른 이름으로 내려왔을 때 수동으로 매칭해준다고 생각하시면 쉬울 것 같습니다.

코드로 예를 들어보면 아래와 같습니다.

```
struct StudentFullname: Codable {
    var firstName: String
    var lastName: String
}

let studentFullname = """
{
    "first_name": "hyunho",
    "last_name": "lee"
}
""".data(using: .utf8)!

let student1Fullname = try! JSONDecoder().decode(StudentFullname.self, from: studentFullname)

print(student1Fullname.firstName)
print(student1Fullname.lastName)

```
실행 해보면 다음과 같은 에러가 발생 합니다.
`No value associated with key CodingKeys(stringValue: \"firstName\", intValue: nil) (\"firstName\").", underlyingError: nil)`
사실 당연합니다. `first_name`를 디코더에 넣고 `firstName`나 `lastName`로 바꾸라고 하면 어떤걸 골라야 할지 사람도 헷갈리 수 있으니까요!

그래서 수동으로 지정해주는 겁니다. 햇갈리지 않게!

```
struct StudentFullname: Codable {
    var firstName: String
    var lastName: String

    enum CodingKeys: String, CodingKey {
        case firstName = "first_name"
        case lastName = "last_name"
    }
}

let studentFullname = """
{
    "first_name": "hyunho",
    "last_name": "lee"
}
""".data(using: .utf8)!

let student1Fullname = try! JSONDecoder().decode(StudentFullname.self, from: studentFullname)

print(student1Fullname.firstName)
print(student1Fullname.lastName)
```
의미를 해석해보면, `first_name`이라는 녀석을 파싱해야 할 때는, `firstName`로 처리 해주면 되겠죠



https://www.hackingwithswift.com/interview-questions/what-does-the-codable-protocol-do

Suggested approach: This protocol was introduced in Swift 4 to let us quickly and safely convert custom Swift types to and from JSON, XML, and similar.

For bonus points talk about customization points such as key and date decoding strategies, the CodingKey protocol, and more, so that you're able to show you can handle a range of input and output styles
