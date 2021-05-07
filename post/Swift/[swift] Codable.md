Codable 프로토콜

API서버와 통신을 하면서 Codable프로토콜을 채택해서 사용해 보았습니다.
JSON의 데이터 구조와 맞아야 하는 것 같다는 느낌만 있어서 어떻게 구성되어있고, 어떻게 사용해야하는지 정리해 놓겠습니다.

먼저 애플의 [공식문서](https://developer.apple.com/documentation/swift/codable)에 들어가서 살펴보겠습니다.  

`Codable is a type alias for the Encodable and Decodable protocols.`  

Encodable과 Decodable을 각각 알아야 하는군요. 그럼 알아보겠습니다.

### Encodable
내가 가진 데이터를 외부의 데이터 타입으로 인코딩 할 수 있게 해주는 프로토콜 입니다. 아래 코드를 가지고 예를 들어보겠습니다.  
데이터를 가지고, json을 만들어 보겠습니다. 데이터를 담을 모델이 필요하겠죠? 간단한 모델을 하나 작성해 줍시다.  
```
struct Fruit {
    var name: String?
    var color: String?
}

let banana = Fruit(name: "Banana", color: "Yellow")
```
Fruit 이라는 모델을 하나 만들고, 데이터를 넣어 즙니다. 이 데이터를 어떻게 json의 형태로 Encoding을 할 수 있을 것인가?  
최족 목표는 아래와 같은 json을 만들어 주는 것 입니다. 무엇을 넣어주면, json으로 만들어 줄 수 있는 걸까요?
```
/// Encodes the given top-level value and returns its JSON representation.
///
/// - parameter value: The value to encode.
/// - returns: A new `Data` value containing the encoded JSON data.
/// - throws: `EncodingError.invalidValue` if a non-conforming floating-point value is encountered during encoding, and the encoding strategy is `.throw`.
/// - throws: An error if any value throws an error during encoding.
open func encode<T>(_ value: T) throws -> Data where T : Encodable
```
Encodable 타입의 데이터를 넣으면, json으로 변환할 수 있군요.  
그럼 우리의 데이터도 Encodable하게 만들어 주어 사용해 봅시다.

```
struct Fruit: Codable {
    var name: String?
    var color: String?
}

let banana = Fruit(name: "Banana", color: "Yellow")
let encoder = JSONEncoder()

if let jsonData = String(data: try encoder.encode(banana), encoding: .utf8) {
    print(jsonData) // {"name":"Banana","color":"Yellow"}
}
```

json의 데이터가 출력 되는 것을 볼 수 있습니다. json의 키 값은 변수명, 밸류값은 변수 값이 되어서 반환 된 것을 알 수 있습니다.

### Decodable
json을 받아서 모델에 데이터를 넣는 방법은 어떻게 될까요? 
위에 했던 방법과 반대로, 디코더를 만들어 주고 타입과 데이터를 넣어주면 디코딩이 됩니다.
```
let jsonData = try encoder.encode(banana)
let decoder = JSONDecoder()
let data = try decoder.decode(Fruit.self, from: jsonData)
if let name = data.name, let color = data.color {
    print(name, color) // Banana Yellow
}
```

디코딩 된 데이터를 사용하시면 끝!
