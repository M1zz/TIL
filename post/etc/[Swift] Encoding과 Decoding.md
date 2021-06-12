iOS개발의 많은 부분들이 데이터를 요청해서 받고, 또 보내는 것인데요. 할 때마다 헷갈려서 정리 해 놓으려고 합니다.


### Data
만약에 아래와 같은 json 데이터를 서버로 보내야 한다고 생각해봅시다.
```
{
  "name" : "Leeo",
  "age" : 29,
  "pet" : {
    "name" : "coco",
    "type" : "dog"
  }
}
```
어떻게 보낼 수 있을까요? 물론 문자열로도 만들 수 있지만, 그러면 같은 모양의 여러 데이터를 여러개 만들 때 비용이 너무 크게 들테니 모델을 만들어서 같은 데이터를 찍어내는게 좋겠습니다. 가장 바깥의 단계 부터 차례대로 구조를 보고 모델을 만들면, 객체를 만들 수 있을 것 같습니다. 아래와 같이 코딩을 하면 변환이 가능해 보입니다!
```
import Foundation

struct Person: Codable {
    var name: String
    var age: Int
    var pet: Animal
}

struct Animal: Codable {
    var name: String
    var type: String
}
```
Codable은 Decodable 과 Encodable을 동시에 채택한 프로토콜입니다. 그러면 Encodable은 뭐고 Decodable은 무엇일까요?


### Encodable
이 프로토콜을 채택하면, 이 구조체는 json데이터로 인코딩 될 수 있음을 보장합니다. Person만 Encodable을 채택하고, Animal은 Encodable을 채택하지 않으면, Person은 json으로 온전히 변환된다는 보장이 없으니, 모두 채택 해 주어야 합니다.
그럼 이제, 우리의 데이터를 json으로 만들어 봅시다.
순서는 다음과 같습니다.

> 인스턴스 -> 인코더에 넣기 -> json 데이터

순서 대로 코딩을 해 보면
```
let friend = Person(name: "Leeo", age: 13, pet: Animal(name: "coco", type: "dog"))
```
인스턴스를 생성해줍니다. 그리고 나서 인코더를 만들고 인코더에 인스턴스를 넣어서 데이터를 만들어 줘야겠죠?
```
// 인코더 만들기
let encoder = JSONEncoder()
// 인코더에 friend 인스턴스 넣어서 jsonData로 만들기
let jsonData = try encoder.encode(friend)
```
그리고 출력을 해보면...엄.. 네 데이터라서 잘 보이지 않습니다. 이 데이터를 문자열로 만들어서 봐주어야 해요.

결과적으로 출력까지 하려면 다음과 같습니다.
```
let encoder = JSONEncoder()
encoder.outputFormatting = .prettyPrinted // jsonFormatting
let jsonData = try encoder.encode(friend)
if let jsonString = String(data: jsonData, encoding: .utf8) {
    print(jsonString)
}
```

위 코드를 출력해보면 결과는 아래와 같습니다.

```
{
  "name" : "Leeo",
  "age" : 13,
  "pet" : {
    "name" : "coco",
    "type" : "dog"
  }
}
```

### Decodable
인스턴스를 json으로 인코딩 했으니 Decodable은 감이 오시죠? 네 우리가 가지고 있는 jsonData를 인스턴스로 만들어주는 변환장치 입니다.

우리가 만든 모델이 Decodable 프로토콜을 채택헀다면, jsonData를 가지고 인스턴스를 만들 수 있습니다. 

순서는 예측 가능하시죠?

> json 데이터 -> 디코더에 넣기 -> 인스턴스

자 그럼 거꾸로 가 봅시다.

```
let myString = """
{
"name" : "Leeo",
"age" : 13,
"pet" : {
  "name" : "coco",
  "type" : "dog"
}
}

let jsonData = myString.data(using: .utf8)
"""
```
문자열을 만들어주시고, jsonData로 변환 해줍니다.

```
if let jsonData = myString.data(using: .utf8) {
    let decoder = JSONDecoder()
    let myFriend = try decoder.decode(Person.self, from: jsonData)
    print(myFriend.name, myFriend.age)
    // Leeo 13

}
```
변환한 데이터를 디코더를 생성해서 넣어줍니다. 타입과, 데이터를 넣어줍니다. 이 타입은 디코딩이 가능한 즉 Decodable 한 타입이어야겠죠? 그러므로 Decodable 프로토콜도 채택 해 줍니다. 중간중간에 decode 메소드가 에러를 발생시킬 수 있기 때문에 try를 쓴 부분들은 이 글에서는 넘어가도록 하겠습니다.

```
import Foundation

struct Person: Decodable {
    var name: String
    var age: Int
    var pet: Animal
}

struct Animal: Decodable {
    var name: String
    var type: String
}
```

### 정리 
우리는 우리가 만든 인스턴스를 json으로 인코딩하는 방법, 즉 변환하는 방법에 대해 알아 보았고, 반대로 전송받은 문자열을 json으로 변환해 인스턴스로 만드는 방법에 대해 알아보았습니다. API요청으로 데이터를 주고 받는 일은 굉장히 자주 일어나는 일이므로 좀 더 연습하고 좋은 예제가 있다면 추가 하도록 하겠습니다.





