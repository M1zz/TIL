# swift 에서 dictionary와 NScache의 차이와 thread safe 하다는 뜻

공부를 하다보니 NScache를 알게 되었습니다. 사실 무엇인지 잘 모르고 사용하고 있었는데, 이 걸 모르는 사람들은 Dictionary를 쓴 경험이 있다고 합니다. 그래서 무엇이 비슷했길래 사용했는지, 그리고 무엇이 다르기에 용도에 맞게 사용해야 하는지 정리를 해보겠습니다.

또한 공부하다 알게 된 thread safe 하다는 것에 대해서도 정리하겠습니다.


## dictionary와 NScache의 비슷한 점

var dictionaryInstance: Dictionary<String, Data> = Dictionary<String, Data>()
var cacheInstance: NSCache<NSString, NSData> = NSCache<NSString, NSData>()

생성을 해보면 정말 비슷해서 아무 생각없이 사용한다면 쓸 수 있겠다라는 생각을 했습니다. 결국 string을 키값으로 데이터를 저장하고 탐색하는데 사용할 수 있기 때문입니다.

그러면 이렇게 비슷한데 그냥 사용하면 무엇이 안 좋은지 한 번 알아보도록 하겠습니다

## dictionary와 NScache의 다른 점

크게 두가지 다른점이 있다고 생각합니다.
- NSCache는 디바이스의 메모리가 부족해지면 캐시된 데이터를 삭제하기도 합니다.
- 가장 큰 차이점 입니다 NScache는 Thread safe 합니다.
- 값의 입력과 출력하는 방법이 조금 다릅니다.(키값으로 접근하는 것은 같습니다.)

그렇다면 가장 큰 차이점인 Thread safe 하다 그렇지 못하다는 것은 어떤 의미일까요?

## Thread safe 하다는 것

해석해보면 thread에 안전하다는 뜻 입니다. 그러면 thread가 무엇인지 우리가 어떤 환경에서 개발하는지 알아야 합니다. thread란 쉽게 말해 작업의 단위라고 보면 쉽습니다. 우리는 multi-threaded 환경에서 개발을 하고 있습니다.

그러면 어려개의 작업은 각각의 thread를 만들고 공유 자원을 이용할 수 있겠죠?

좀 더 쉽게 예를 들어보겠습니다.

냉장고가 하나 있고, A와 B가 물건을 넣기도 빼기도 한다고 해봅시다. 현실에서의 냉장고는 동시에 물건을 넣거나 뺼 수가 없습니다. 그래서 이전에 A가 빼간 물건은 당연히 B가 그 다음에 냉장고를 열었을 때 없고 B가 넣은 물건은 A가 다시 열었을 때 잘 들어있습니다. 그런데 multi-threaded 환경에서는 다릅니다.

A와 B는 하나의 냉장고를 쓰고 있다고 생각하지만 thread-safe 하지 않을 때는 A와 B가 동시에 냉장고를 열면 안에 같은 물건이 있는 것을 볼 수 있고, 또한 동시에 물건을 꺼낼 수 있습니다. 그렇다면 동시에 냉장고를 열고, 한개의 사과에 대해 한명은 절반을 잘라 다시 넣고, 다른 한 사람은 한입을 먹고 넣었을 때, 그 다음에 다시 열면 어떤 모양 이어야 할까요? 내가 잘랐던 절반의 사과가 한 입 베어 물린 상태가 되어있는 것을 본다면 그 다음부터는...헬게이트!

그래서 multi-threaded 환경에서는 공유자원을 thread-safe 하게 사용할 수 있게 해야합니다.

NSCache는 여러 thread에서 수정을 해도 thread-safe 하게 동작한다는 것이 바로 이런 의미입니다.

좀 더 개발자 스럽게 코드로 예를 보여드리겠습니다

```
import Foundation

struct AppleBox {
    static var amount = 10
}

struct Person {
    let name: String

    func eat(amount: Int) {
        print("\(name): starts to eat apples...")
        if AppleBox.amount > amount {
            // sleeping for some time, simulating server process
            Thread.sleep(forTimeInterval: Double.random(in: 0...1))
            AppleBox.amount -= amount
            print("\(name): eats \(amount) apples")
            print("current amount is \(AppleBox.amount)")
            if AppleBox.amount < 0 {
                print("there is no apple!")
            }
        } else {
            print("\(name): Can't eat because there is no apple!")
        }
    }
}

func tryRaceCondition() {
    let leeo = Person(name: "leeo")
    let batgirl = Person(name: "batgirl")
    let queue = DispatchQueue(label: "eatQueue", attributes: .concurrent)

    queue.async {
        leeo.eat(amount: 8)
    }

    queue.async {
        batgirl.eat(amount: 8)
    }
}

tryRaceCondition()
```

위 코드를 보시면 `leeo`와 `batgirl`이 각각 8개의 사과를 꺼내어 먹습니다. 그리고 그 위에 `Person`은 AppleBox에서 사과를 `eat()` 할 수 있습니다. 그리고 먹을 때는 남아있는 수량을 확인하고 먹을 수 있으면 먹습니다. 하지만 결과는 아래와 같습니다.

```
leeo: starts to eat apples...
batgirl: starts to eat apples...
batgirl: eats 8 apples
current amount is 2
leeo: eats 8 apples
current amount is -6
there is no apple!
```
leeo와 batgirl은 각각 사과를 먹을 때만해도 수량이 남아서 사과를 먹을 수 있었습니다. 하지만 결과는 말도 안돼게 -6의 사과가 남았습니다. 0보다 더 적은 숫자가 남다니요...

위와 같이 여러개의 쓰레드에서 공유자원에 동시에 접근하면 우리가 짜놓은 로직이 정상으로 동작하지 않을 때가 있습니다. 만약 thread-safe 하게 작성되었다면 둘 중 한명은 사과를 먹지 못했겠죠?

## 정리

NSCache를 공부하다가 dictionary와의 공통점 차이점을 공부하게 되었습니다. 그리고 NSCache를 써야하는 이유인 thread-safe 하지 않으면 생기는 일에 대해서도 살펴보았습니다. 이렇게 정리하고나니 저 둘은 전혀 비슷해보이지 않네요!
