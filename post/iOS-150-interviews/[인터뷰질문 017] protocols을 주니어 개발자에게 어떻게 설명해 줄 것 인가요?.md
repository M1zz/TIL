# [인터뷰질문 017] protocols을 주니어 개발자에게 어떻게 설명해 줄 것 인가요?

관련주제 : Swift
난이도 : 하

## 프로토콜이란?
프로토콜은 인터페이스 입니다. swift에서는 특정 기능을 정의합니다. 실제 구현은 하지 않고 정의만 합니다. 마치 설계도와 같죠.

## 간단한 예제
간단한 예제를 보도록 하겠습니다.
자동차의 기능을 구현하기에 앞서 일단 정의하려고 하는데요. 자동차가 주행에 필요한 것들을 나열해 봅시다.
간단하게 생각해서 출발과, 정지를 할 수 있으면 자동차가 주행할 수 있겠죠?
앞에 나오는 조건들이 주행에 필요한 요소들이라고 했으니 이 조건들을 모두 갖추면, 주행이 가능하다고 할 수 있습니다. 그럼 한번 설계 해 볼까요?
```
protocol Driveable {
    func start()
    func stop()
}
```
이렇게 설계 해 놓으면 이 프로토콜을 채택하면, 아 주행가능하구나 라고 생각하면됩니다. 아래와 같이 구현되겠죠?
```
struct Sonata: Driveable {
    func start() {
        print("주행 시작합니다.")
    }

    func stop() {
        print("주행 종료합니다.")
    }
}
```
사실 위에서 Driveable 프로토콜을 채택하지 않아도 돌아가는데는 문제가 없습니다. 하지만 다른 곳에서 사용할 때 프로토콜을 채택하고 있지 않으면 주행이 가능한건지에 대한 보장이 되지않겠죠? 그렇기 때문에 설계와 기능의 보장을 위해 프로토콜을 정의해서 채택해서 써야합니다.

## 여러 프로토콜
하나의 프토콜이 한 종료의 기능을 보장해준다면, 여러개의 기능은 아래와 같이 여러개의 프로토콜을 채택해서 구현할 수 있을 것 입니다. 거꾸로 이 객체가 어떤 기능을 하는지에 대한 것들은 프로토콜을 살펴보는 것 만으로도 가능 합니다.
```
protocol Driveable {
    func start()
    func stop()
}

protocol Openable {
    func open()
    func close()
}

struct Sonata: Driveable {
    func start() {
        print("주행 시작합니다.")
    }

    func stop() {
        print("주행 종료합니다.")
    }
}

struct BMW: Driveable, Openable {
    func start() {
        print("부르르릉 주행 시작합니다.")
    }

    func stop() {
        print("끼이이익 주행 종료합니다.")
    }

    func open() {
        print("차 뚜껑 열겠습니다.")
    }

    func close() {
        print("차 뚜껑 닫겠습니다.")
    }
}

let myCar = Sonata()
myCar.start() // 주행 시작합니다.
myCar.stop() // 주행 종료합니다.

let dreamCar = BMW()
dreamCar.start() // 부르르릉 주행 시작합니다.
dreamCar.open() // 차 뚜껑 열겠습니다.
```

## 정리
프로토콜은 구현하기 전에, 구현체가 어떤 기능을 가져야 하는가에 대한 설계도 입니다. 만약에 이미 정의된 기능이 있다면 채택해서 구현하면 되고, 필요하다면 새로 정의해서 채택할 수도 있습니다.
