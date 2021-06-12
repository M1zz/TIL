GCD를 공부하던 중 정리가 잘 되지 않아 정리 해 보려고 합니다. 그리고 같이 공부할 키워드로 떠오른 Operation Queue와는 어떤점이 다른지 정리 하려고 합니다.  
GCD(Grand Central Dispatch)는 멀티 코어 하드웨어에서 코드 동시 실행을 지원합니다.

코드의 동시실행을 이해하려면, 실행의 단위를 잘 알아야 합니다.

코드를 작성하면, 어떠한 기능을 하는 파일이 됩니다. 이를 프로그램 이라고 합니다.  
이 프로그램은 운영체제의 메모리에 올라가며, 프로그램의 인스턴스를 생성합니다.

이를 `프로세스`라고 합니다.
프로세스는 운영체제로 부터 독립된 자원을 할당 받습니다.

그리고 그 프로세스에서 여러가지의 일을 실행하는 단위를 `쓰레드` 라고 합니다.
쓰레드는 프로세스가 할당 받은 자원 중 독립적으로 스택만 새로 할당 받고, 나머지 자원들은 쓰레드끼리 공유합니다.

정래 해보면 코드로 프로그램을 실행하고, 그 프로그램이 하나 실행되면 하나의 프로세스가 생성됩니다. 그리고 그 프로세스 안에서 흐름의 단위가 쓰레드 입니다.

### 멀티 프로세스 와 멀티 쓰레드
이름으로 부터 의미를 유추 해 보도록 하겠습니다.
멀티 프로세스란 하나의 프로그램 안에 어려가의 프로세스가 동작할 수 있도록 하는 것 이겠군요. 그렇다면 
하나의 프로그램 안에서 각각의 독립적인 자원을 할당받은 프로세스들이 기능을 할테니, 프로세스끼리 영향을 주지 않아 독립적으로 사용할 수 있어서 좋겠습니다. 하지만 반대로 공유하는 자원이 없어서, 데이터를 전송하고 받아야겠네요.

멀티 쓰레드란 하나의 프로세스에서 여러개의 쓰레드를 사용하는 방법 일 것 입니다. 멀티 프로세싱의 방법과 반대로, 공유하는 자원이 있으며, 데이터를 전달하는 방법이 간단할 것 같습니다. 하지만 공유하는 자원이 있따는 것은 잘 못 관리하면 의도하지 않았던 데이터의 왜곡이 일어나서 버그를 만들기 쉽겠습니다.

### 동시성 과 병렬성
동시성(Concurrency) 과 병렬성(Parallelism) 이라는 용어에 대해 정리 해 보도록 하겠습니다. 
얼핏 보면 같은 의미인 것 같으나 용어가 다른만큼 다른 의미를 가지고 있습니다. 

동시성은 동시에 실행되는 것 처럼 보이는 것이다. 예를 들면 주방장이 한 명인 식당에 스테이크 4개를 주문했을 때, 스테이크1, 스테이크2, 스테이크3, 스테이크4를 순서대로 만들지 않습니다. 네 개의 고기를 다 올리고, 적당히 뒤집으면 4개를 동시에 요리할 수 있고, 실제로 동시에 요리 하는 것 처럼 보입니다. 하지만 요리사는 1명이기에 4개의 스테이크를 동시에 요리하고 있지는 않습니다.

병렬성은 실제로 동시에 실행되고 있는 것처럼 뜻합니다. 위의 예를 다시한번 들면, 주방이 2개인 식당에 스테이크를 4개 주문하면 각각 2개씩 만들면 어느 한 순간, 사진을 찍었을 때 실제 2명이 동시에 요리를 하고 있는 것 처럼 보이겠죠? 동시성을 가진 사진에는 언제나 1명의 요리사 만이 등장합니다.

### GCD(Grand Central Dispatch)
멀티 코어 하드웨어에서 동시성 코드 실행을 지원하는 방법입니다. 프로그래머가 작업을 생성하고 Dispatch Queue에 추가하면, GCD가 쓰레드를 생성해 실행합니다. 실행이 완료된 후에는 쓰레드가 제거됩니다.

### DispatchQueue
해야 할 작업을 담아두는 Queue입니다. 큐의 대기열에 올라간 아이템은 시스템에 의해 관리되는 스레드 풀에이 처리해 줍니다. 큐는 First In First Out의 구조를 가진 자료구조로 먼저 들어간 작업은 먼저 처리됩니다. 

DispatchQueue에는 크게 두 종류가 있는데 하나는 Serial Queue이고 나머지는 Concurrent Queue 입니다. 

- Serial Queue : 이전 작업이 끝나면 다음 작업을 순차적으로 실행하는 직렬 형태의 Queue입니다.
- Concurrent Queue : 병렬 형태로 실행되는 Queue 입니다.

당연한 이야기지만 sync한 작업은 순서가 보장이되고, async한 작업은 순서가 보장되지 않습니다.

### Practice

실제 ViewDidLoad에 함수를 호출하여 어떤 순서대로 출력되는지 확인 해 보도록 하겠습니다. 문제를 보고 출력 결과를 예측 해 보세요!
sync는 큐에 작업을 추가하고 작업의 종료를 기다립니다. 하지만 async는 큐에 작업을 추가만 할뿐 작업의 종료를 기다리지는 않습니다.

1번 문제
```
private func syncTest() {
    DispatchQueue.global().sync { print("1") }
    print("2")
    DispatchQueue.global().sync { print("3") }
    print("4")
    DispatchQueue(label:"SerialQueue").sync { print("5") }
    print("6")
}
```
1번 결과 
> 1, 2, 3, 4, 5, 6 입니다.

1번 해설

1번이 큐에 들어가고 종료를 기다렸다가, 2번을 실행하고 다시 3번을 큐에 추가해서 종료를 기다렸다가 4번을 실행하고, 5번 SerialQueue 라는 큐에 작업을 넣었다가 6번까지 출력합니다. 그러므로 결과는 



2번 문제
```
private func asyncTest() {
    DispatchQueue(label:"SerialQueue").async { print("1") }
    print("2")
    DispatchQueue(label:"SerialQueue").async { print("3") }
    print("4")
    DispatchQueue(label:"SerialQueue").async { print("5") }
    print("6")
}
```
2번 결과
> 2, 1, 4, 3, 6, 5

2번 해설

결과 값은 실행 결과 값은 실행 때마다 달라지지만, 1 작업을 먼저 async 큐에 넣고 끝나기를 기다리지 않고, 2를 출력합니다. 그리고 다시 3을 async 큐에 넣고, 4를 실행 마찬가지로 5도 async 큐에 넣고 6이 실행됩니다. 즉 1, 3, 5 는 2, 4, 6 과 async하게 작업되므로, 각각의 순서는 보장되나 실행 할 때마다 결과가 다르게 나옵니다.

3번 문제

```
private func asyncConcurrentTest() {
    DispatchQueue.global().async { print("1") }
    print("2")
    DispatchQueue.global().async { print("3") }
    print("4")
    DispatchQueue.global().async { print("5") }
    print("6")
}
```
3번 결과
> 2, 1, 4, 6, 3, 5

3번 해설  

2, 4, 6의 순서는 언제나 보장됩니다. 하지만 1, 3, 5에 대해서 순서가 보장되지 않습니다. 멀티 쓰레드 형식으로 동작하기 때문에 어떤 작업이 먼저 끝날지 모릅니다.


### Main Queue 와 Global Queue
앱이 실행되면 시스템에서 기본적으로 이 2개의 큐를 만들어 줍니다.

- Main Queue : 메인 쓰레드에서 UI를 위해 사용하는 Serial Queue 입니다. 모든 UI처리는 메인 쓰레드에서 처리해야 합니다. 그러므로 이 메인 쓰레드에 Task가 많아지면 앱이 버벅일 수 있습니다.
- Global Queue : 편리하게 사용할 수 있게 만들어 놓은 Concurrent Queue 입니다. Global Queue는 우선 처리되어야 할 것들의 순위를 표시하기 위해 qos(Quality of service) 파라미터를 제공합니다. 병렬적으로 동시에 처리하기 때문에 작업 완료의 순서는 보장할 수는 없지만 일의 우선순위를 높일 수 있습니다.

QOS 우선순위는 아래와 같습니다.
1. userInteractive : 중요도가 높고 즉각적인 반응이 요구되는 작업 처리. UI 업데이트나 이벤트 핸들링 등에 사용.  
2. userInitiated : 빠른 결과를 기대할 때 사용하는 QoS  
3. default : 기본 값  
4. utility : 계산, I/O, 네트워킹 등 시간이 다소 오래 걸리는 작업  
5. background : 유저가 인지하지 못하는 뒷단에서 실행되는 작업  
6. unspecified  

### Dispatch Source
특정 유형의 시스템 이벤트를 비동기적으로 처리하기 위한 C 기반 매커니즘입니다.  
특정 유형의 시스템 이벤트에 대해 정보를 캡슐화하고, 해당 이벤트가 발생할 때마다 특정 클로저 객체 혹은 기능을 Dispatch Queue에 전달합니다.


### Operation Queue
Operation : 작업과 관련된 코드와 데이터를 나타내는 추상 클래스입니다  
Operation Object : 애플리케이션에서 수행하려는 Operation을 캡슐화하는 데 사용하는 Foundation 프레임워크의 Operation 클래스 인스턴스입니다.  

Operation Queue
 - Operation의 실행을 관리하며, Operation을 담는 큐(Queue)
 - 큐에 담긴 Operation을 취소하려면 Operation Object의 cancel()을 호출하거나 Operation Queue의 cancelAllOperations()를 호출하여 대기열의 모든 연산을 취소하는 방법이 있습니다.
 - KVO를 사용해 작업 진행 상황을 감시할 수 있습니다.
Operating Queue의 좀 더 많은 사용법을 알고 싶은신 분은 [Concurrency Programming Guide](https://developer.apple.com/library/archive/documentation/General/Conceptual/ConcurrencyProgrammingGuide/Introduction/Introduction.html)를 참고하시면 됩니다.

### 결론 
GCD vs Operation Queue  
- Operation Queue에서는 동시에 실행할 수 있는 Operation의 최대 수를 지정할 수 있습니다.  
- Operation Queue에서는 Operation을 일시 중지, 다시 시작 및 취소를 할 수 있습니다.
- Operation Queue에서는 KVO를 사용할 수 있는 많은 프로퍼티들이 있습니다.
 > isCancelled, isAsynchronous, isExecuting, isFinished, isReady, dependencies, queuePriority, completionBlock  

- GCD는 앞선 기능이 없습니다. Operation Queue를 좀 더 쉽게 쓸 수 있도록 개발한 것으로 좀 더 오버헤드가 있지만 사용하기에 매우 간편합니다. 













