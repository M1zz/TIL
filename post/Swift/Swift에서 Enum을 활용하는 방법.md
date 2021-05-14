# Swift에서 Enum을 활용하는 방법

개발을 하고, 리팩토링을 하다보면 Enum을 썼을 때 좋은 결과를 얻은 경험이 많습니다. 그러나 너무 남발하게되면 또 코드가 더러워져서 어떻게 응용하면 좋을지 정리해 보도록 하겠습니다.

- enum이 무엇인지 문법을 알지만 어떻게 활용해야 할지 모르는 분들
- 미래의 나를 위해 작성했습니다

## 프로퍼티 활용방법
enum에는 `stored properties`를 만들 수 없습니다 즉 변수를 선언해서 할당할 수 없다는 뜻이죠. 하지만 `computed properties`는 선언해서 활용할 수 있습니다.
예제 코드를 보여드릴게요
```
enum Food {
    case korean
    case japanese

    var name: String {
        switch self {
        case .korean:
                return "kimchi"
        case .japanese:
                return "sushi"
        }
    }
}

let todaysLunch = Food.korean.name
print(todaysLunch)
// kimchi
```
위와 같이, 음식의 종류를 정했다면, 저장 프로퍼티인 `name`을 만들어서 각각의 값을 저장 해 줄 수 있습니다. 저는 enum에 String을 지정해서 return을 해주려고 했었는데 이 방법이 훨씬 깔끔한 것 같아요. 각각에 변수마다 필요한 값들을 내려주는 것이 명확하네요

## 메소드 활용방법
프로퍼티 말고 메소드도 enum에 정의해서 활용할 수 있습니다. 아래의 예시를 통해 설명 드리도록 하겠습니다.
```
enum Food {
    case korean
    case japanese

    func description() -> String {
        switch self {
        case .korean:
            return "\(self)은 한식입니다."
        case .japanese:
            return "\(self)은 일식입니다."
        }
    }
}

let todaysLunchDescription = Food.korean.description()
print(todaysLunchDescription)
// korean은 한식입니다.
```
메소드를 추가하여 분기를 태워 각각에 필요한 동작을 하는 코드를 짜서 활용하는 방법도 있습니다.

## enum안의 enum 활용방법
enum안에 enum이 왜 필요할까요? 케이스를 외우고 찾으려 하니 잘 생각 나지 않아서, 근본적으로 enum이 필요한 경우를 생각해 보았습니다.
일반적으로 선택지가 제한되어 있을 때, 저는 enum을 사용합니다. 위에서도 음식이 한, 중, 일, 양식만 있다면, 굳이 여러개의 입력을 받는것이 아닌, 선택지 중에 선택을 한다면 혼동이 덜 할 것 입니다.
그렇다면 다시 처음의 질문으로 돌아가서 선택지 안에 또 선택지가 있다면, 우리는 enum안에 enum을 넣는 선택을 할 것입니다.

```
enum Character {
    enum Weapon {
        case bow
        case sword
        case axe
    }

    enum Armor {
        case chain
        case wooden
        case leather
    }

    case wrriror(weapon: Weapon, armor: Armor)
    case archor(weapon: Weapon, armor: Armor)

    func description() -> String {
        switch self {
        case let .wrriror(weapon, armor):
            return "검사의 무기는 \(weapon) 갑옷은 \(armor) 입니다"
        case let .archor(weapon, armor):
            return "검사의 무기는 \(weapon) 갑옷은 \(armor) 입니다"
        }
    }
}

let myCharactor = Character.archor(weapon: .bow, armor: .chain)
let supporterChartor = Character.wrriror(weapon: .sword, armor: .leather)

print(myCharactor.description(), supporterChartor.description())
//검사의 무기는 bow 갑옷은 chain 입니다 검사의 무기는 sword 갑옷은 leather 입니다
```
선택지에 갈림길이 있다면, enum안에 enum을 넣어 놓으면 코딩할 때 .만 찍어서 로직을 짤 수 있는 편리함이 있었습니다.

다만 수정할 떄 어디를 수정해야 하는지를 파악하는데 굉장히 힘들었습니다.

## struct와 class에서 enum 활용하기
이전의 코드에서는 enum안에 enum을 넣어서 정리를 했습니다. 하지만 마지막에도 언급했듯이 무언가가 중첩되는 것은 이해하기 힘들고 또 헷갈리기 쉽습니다. 이럴 때 활용하면 좋은 방법입니다.

```
struct Character {
    enum CharacterType {
        case thief
        case warrior
    }

    enum Weapon {
        case bow
        case sword
        case dagger
    }

    enum Armor {
        case chain
        case wooden
        case leather
    }

    let type: CharacterType
    let weapon: Weapon
    let armor: Armor
}

let myCharacter = Character(type: .warrior, weapon: .sword, armor: .leather)
//warrior has sword and leather
```
이런식으로 선택할 수 있는 값들이 있다면, 클래스나 구조체 안에서 정의하고 사용하는 사람은 값을 입력하는게 아닌, 선택해서 사용할 수 있게 만들면 좋을 것 같습니다.


## Mutating Method 활용방법
enum으로 정의된 타입을 변경할 수 있는 메소드를 작성해서 상태값을 변경해 줄 수 있습니다. struct나 enum의 값을 변경 해줄 때는 mutating 키워드를 붙여야 합니다. 간단한 방법이니 아래 코드를 통해서 이해해보도록 하겠습니다.
```
enum BatteryStateSwitch {
    case discharge
    case low
    case high
    case full
    mutating func charge() {
        switch self {
            case .discharge:
                self = .low
            case .low:
                self = .high
            case .high:
                self = .full
            default:
                print("boom")

        }
    }
}

var batteryState = BatteryStateSwitch.discharge
print(batteryState) // discharge
batteryState.charge()
print(batteryState) // low
batteryState.charge()
print(batteryState) // high
batteryState.charge()
print(batteryState) // full
batteryState.charge() // boom
```

## Static Method 활용방법
enum안에 static method를 정의하여 활용하는 방법도 있습니다. 활용 케이스는 struct에서 static method가 필요한 경우와 같을 것 같습니다. 아래는 이름을 입력해서 디바이스의 타입을 반환 받는 메소드 입니다.
```
enum Device {
    case iPhone
    case iPad

    static func getDevice(name: String) -> Device? {
        switch name {
            case "iPhone12":
                return .iPhone
            case "iPhone8":
                return .iPhone
            case "iPad":
                return .iPad
            case "iPadPro":
                return .iPad
            default:
                return nil
        }
    }
}

if let device = Device.getDevice(name: "iPhone12") {
    print(device)
    //prints iPhone
}

```

## Custom Init 활용 방법
enum은 기본적으로 init이 필요하지 않습니다. 하지만 init을 구현하여 원하는 타입을 반환하도록 할 수 있습니다. 마치 값을 입력받아 해당하는 선택을 반환하도록 말이죠.

```
enum HTTPStatus {
    case continueStatus
    case ok
    case multipleChoice
    case badRequest
    case internalServerError
    case error

    init(statusCode: Int) {
        switch statusCode {
        case 100..<200 :
            self = .continueStatus
        case 200..<300:
            self = .ok
        case 300..<400:
            self = .multipleChoice
        case 400..<500:
            self = .badRequest
        case 500..<600:
            self = .internalServerError
        default:
            self = .error
        }
    }
}

let networkStatus = HTTPStatus(statusCode: 404)
print(networkStatus)
//badRequest

```

## Protocol Oriented Enum
enum이 프로토콜을 채택해서, 프로토콜 지향적으로 사용되는 방법입니다. 전혀 들은 적도 없고 고민해 본 적도 없기 때문에
