### strong, weak, unowned 사용 방법
클래스의 객체를 가리키는 참조의 기본값은 강한 참조이다. 그리고 적어도 하나의 강한 참조가 있다면 객체의 메모리는 ARC(Automatic reference counting)에 의해서 해제되지 않는다.  
아래 예제를 통해 살펴보자
```
// Case1 : 일반 참조와 해제
class TestClass {
    init(){
        print("init")
    }
    deinit{
        print("deinit")
    }
}

var testClass: TestClass? = TestClass()
print("Instance is created!")
testClass = nil
```
```
// print
init
Instance is created!
deinit

```
TestClass 객체가 생성되면서 `init`을 출력한다. 그 다음에 `"Instance is created!"`를 출력한다. `testClass`변수에 nil을 할당하면서 참조의 갯수가 0이 되고 `deinit`을 출력한 뒤 메모리에서 해제된다.

다음 예제를 통해 ARC가 동작하지 않아 메모리에서 해제되지 않는 상황을 살펴보자.
```
// Case 2 : 강한 상호참조
class TestClass{
    var testClass: TestClass? = nil
    init(){
        print("init")
    }
    deinit{
        print("deinit")
    }
}

var testClass1: TestClass? = TestClass()
var testClass2: TestClass? = TestClass()

testClass1?.testClass = testClass2
testClass2?.testClass = testClass1

testClass1 = nil
testClass2 = nil
```

```
// print 
init
init
```
코드의 마지막 부분에 각각의 인스턴스를 참조하고 있는 변수에 nil을 할당해 참조를 끊었지만, testClass1 인스턴스와 testClass2 인스턴스가 서로 강한 참조를 하고 있기 때문에 참조의 갯수가 0이 되지 않아 메모리에서 해제되지 않았다.
이러한 현상을 메모리 누수(Memory leak)라고 한다. 어플리케이션에서 메모리 누수가 일어나면 사용되는 메모리의 양이 늘어난다. 어떻게 하면 이러한 일을 막을 수 있을까?

```
// Case 3 : 약한 상호참조
class TestClass{
    weak var testClass: TestClass? = nil
    
    init(){
        print("init")
    }
    deinit{
        print("deinit")
    }
}

var testClass1: TestClass? = TestClass()
var testClass2: TestClass? = TestClass()

testClass1?.testClass = testClass2
testClass2?.testClass = testClass1

testClass1 = nil
testClass2 = nil
```

```
// print
init
init
deinit
deinit
```
2번째 케이스와 같은 코드이지만 참조에 `weak`을 선언한다면, 강한 참조가 아닌 약한 참조가 된다. 참조는 할 수 있지만 강한 참조가 아니기 때문에 RC(Reference counting)가 늘어나지 않는다.
객체의 메모리가 해제되면 그에 대응하는 변수에는 nil값이 들어간다. 만약 변수가 메모리가 해제된 구역을 가리키고 있다면, runtime error를 발생시킬 것 이다. 그러므로 약한 참조 변수는 반드시 Optional 이어야한다.

순환 참조를 피하는 방법에는 `weak` 말고도 `unowned`가 있다. 두 키워드의 다른점은 unowned로 선언된 변수에는 `nil`값이 들어갈 수 없다. 즉 앞에서 설명했듯이 메모리가 해제된 영역을 가리킬 수 있고 이는 runtime error를 일으킬 수 있다. 그러므로 `unowned`를 쓰려면 객체의 메모리가 해제된 이후 해당 영역을 가리키지 않는다는 확신이 있어야 한다.  

즉 항상 값을 가지고 있어야 하는 변수의 경우에는 `unowned`를 사용하고, 값이 있을지 없을지 모르는 'Optional'의 경우에는 'weak'를 사용하여 순환참조를 피할 수 있다.

### Retain Cycle의 일반적인 시나리오 : Delegates 
```
class ParentViewController: UIViewController, ChildViewControllerProtocol: class{
    let childViewController = ChildViewController()
    func prepareChildViewController(){
        childViewController.delegate = self
    }
}

protocol ChildViewControllerProtocol {
    // important functions
}

class ChildViewController: UIViewController {
    var delegate: ChildViewControllerProtocol?
}
```
Retain Cycle이 일어나는 흔한 시나리오는 delegate 패턴에서 볼 수 있다. 부모 뷰 컨트롤러는 특정 상황에서의 정보를 얻기 위해 본인을 자식 뷰 컨트롤러의 대리자로 설정한다. 이 때 부모 뷰 컨트롤러가 pop 된 이후에 Retain cycle이 발생하여 메모리 누수가 일어난다. 이를 막기 위해 `weak var delegate: ChildViewControllerProtocol?`으로 선언해 주어야 한다.

### Retain Cycle의 일반적인 시나리오 : Closures  

```
class TestClass{
    var aBlock: (()->())? = nil
    let aConstant = 5
    
    init(){
        print("init")
        aBlock = {
            print(self.aConstant)
        }
    }
    deinit{
        print("deinit")
    }
}

var testClass: TestClass? = TestClass()
testClass = nil
```

위의 코드에서 TestClass 객체의 메모리가 해제되지 않는다. TestClass의 객체의 내부에서 Closure로, Closure에서 TestClass객체로 강한 참조를 하고 있기 때문이다. 이러한 문제를 weak self를 capture해줌으로써 해결할 수 있다.

```
class TestClass{
    var aBlock: (()->())? = nil
    let aConstant = 5
    
    init(){
        print("init")
        aBlock = { [weak self] in
            print(self?.aConstant)
        }
    }
    deinit{
        print("deinit")
    }
}

var testClass: TestClass? = TestClass()
testClass = nil
```

### Retain cycles 을 deinit 문 안 메시지 출력으로 찾아내자
```
deinit{
    print("deinit")
}
```
객체가 메모리에서 해제될 때 호출되는 deinit 문에서 메시지를 출력해 준다면, 어느 객체가 메모리에서 해제되지 못하고 retain cycle이 발생했는지 탐지할 수 있다. 


참고 : http://www.thomashanning.com/retain-cycles-weak-unowned-swift/  
참고 : http://minsone.github.io/mac/ios/rules-of-weak-and-unowned-in-swift
