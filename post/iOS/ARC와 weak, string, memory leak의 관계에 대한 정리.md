# ARC와 weak, strong, memory leak의 관계에 대한 정리

면접 질문에도 자주 등장하고, 스스로도 코딩을 하면서 자주 놓치게되는 부분이기에 실제 어떻게 사용해야하는지 되새길 겸 정리하려고 합니다.

먼저 단어들에대서 설명하고, 어떤 문제가 생길 수 있기 때문에 알고있어야 하는지 그리고 어떻게 해결할 수 있는지에 대해 순서대로 정리하도록 하겠습니다.

## ARC(Automatic Reference Counting)
이름에서 알 수 있듯이 자동으로 참조하고있는 숫자를 세어주는 녀석입니다. 그렇다면 참조숫자는 왜 필요할까요?

```
메모리1 - [객체1|객체2|객체3|객체1을바라봄|객체1을바라봄|객체2를바라봄|객체3을바라봄|||빈공간들||]
```
위와 같이 메모리를 사용하고 있는데 객체1이 메모리에서 사라지면 어떻게 될까요? 참조하러 갔는데 비어있으면 크래시가 나겠죠? 아니면 예상하지 못한 동작을 할 것 입니다.

그러므로 지금 메모리에 올라가 있는 이 녀석을 지워도 되는지 말아야 하는지에 대한 기준을 바로 ARC로 잡는 것입니다. ARC가 0이라면? 아무도 참조하고 있지 않으니 메모리에서 해제해도 아무런 이상이 없을 것 입니다.
반대로 0이 아니라면 어디선가, 누군가인지는 모르지만 사용하고 있으니 해제하면 안된다는 뜻으로 알고 메모리에 남겨둡니다.

개인적으로는 지하주차장 천장에 있는 녹색등과 초록등으로 이 아래에 차가 있는지 없는지 알려주는 장치와 비슷하다고 생각했습니다. 이 장치가 완벽하게 동작한다면 주차가 되어있다고 빨간불이 들어와있는 구역에 주차하러 왔다가 낭패를 보는 일은 없을테니까요. ㅎ ㅎ 메모리에서 해제해도 돼, 안돼!

그러면 ARC를 증가시키는 방법을 간단하게 살펴보겠습니다.

```
import Foundation
class Student {
    let name: String
    let grade: Int

    init(name: String, grade: Int) {
        self.name = name
        self.grade = grade
    }

    deinit {
        print("good bye!")
    }
}


print("leeo arc : \(CFGetRetainCount(Student(name: "leeo", grade: 3)))") // 1

var leeo: Student? = Student(name: "leeo", grade: 3)
print("leeo arc : \(CFGetRetainCount(leeo))") // 2

var frank = leeo
print("leeo arc : \(CFGetRetainCount(leeo))") // 3

var gilly = leeo
print("leeo arc : \(CFGetRetainCount(leeo))") // 4

gilly = nil
print("leeo arc : \(CFGetRetainCount(leeo))") // 3
```
위의 코드를 플레이그라운드에서 돌려보면서 꼭 어떻게 돌아가는지 이해해 봅시다 :)

## weak, strong
참조를 공부하다보면 항상 이 키워드들을 마주치게 됩니다. 기본적으로 우리가 변수를 선언하면 `strong`으로 선언이 됩니다. 그리고 `weak`키워드를 앞에 붙이면 weak로 선언되게 됩니다.

자 그러면 이 두개의 차이가 무엇인지 알아봅시다. 아주 명백한 차이는 바로 ARC를 증가시키는지에 대한 여부 입니다. 즉 `strong`으로 메모리를 참조하면 ARC가 1증가합니다. 위에서 보았던 예제가 바로 그렇습니다.

반대로 `weak`를 붙이면 ARC가 증가하지 않습니다. ARC를 증가시키지 않기 때문에, 메모리 해제에 영향을 주지 않게되겠죠?

아래의 코드로 어떤 차이가 있는지 살펴봅시다.

```
print("leeo arc : \(CFGetRetainCount(Student(name: "leeo", grade: 3)))") // 1

var leeo: Student? = Student(name: "leeo", grade: 3)
print("leeo arc : \(CFGetRetainCount(leeo))") // 2

weak var frank = leeo
print("leeo arc : \(CFGetRetainCount(leeo))") // 2

weak var gilly = leeo
print("leeo arc : \(CFGetRetainCount(leeo))") // 2

gilly = nil
print("leeo arc : \(CFGetRetainCount(leeo))") // 2
```

## memory leak
`memory leak`은 간단히 설명하면 메모리가 필요해서 할당해 사용한다음에, 해제가 되지 않는 경우를 말합니다. 다 사용하면 해제를 해 주어야 또 사용할 수 있는데 해제가 안되기 때문에 나중에 가면 사용할 수 있는 메모리가 모자라게 되겠죠? 메모리 릭은 메모리 부족현상을 발생시키는 원인중 하나 입니다.

## arc와 memory leak의 관계
강한 참조가 메모리에서 어떻게 해제하지 못하게 하는지에 대해서 부터 알아보겠습니다.

```
class Student {
    let name: String
    let grade: Int

    init(name: String, grade: Int) {
        self.name = name
        self.grade = grade
    }

    deinit {
        print("good bye!")
    }
}

var leeo: Student? = Student(name: "leeo", grade: 3)
var frank = leeo
var gilly = leeo
leeo = nil
```

leeo는 Student를 참조했다가 더 이상 사용하지 않아서, 해체를 했습니다. 하지만 그래도 deinit의 good bye는 불리지 않죠. 메모리에서 해제되지 않았다는 뜻 인데요, 왜 일까요? 바로 아랫줄에 frank, gilly가 메모리를 참조하고 있기 때문에 사용되고 있다고 판단되어 해제되지 않습니다.

이런 경우는 당연히 해제가 되면 안돼는 케이스이고 잘 쓰고 있는케이스 입니다. 결국에 `frank = nil, gilly = nil`을 하게 되면 메모리에서 해제가 될테니까요.

그렇다면 어떻게 하면 해제도 안돼면서 사용만하게 될까요?
해결방법은 생각보다 간단합니다.
1. 강한 순환참조가 일어나지 않게 설계를한다
2. 강한 순환참조가 일어나는 부분을 약한 순환참조로 구현해준다.

아래의 예제는 2번의 방법을 사용한 것입니다. `weak`가 무슨일을 하는지 아직도 잘 이해가 되지 않으신다면 위에서 부터 다시 천천히 읽어보시길 권해드립니다.
```
import Foundation
class Student {
    let name: String
    var grade: Grade?

    init(name: String) {
        self.name = name
    }

    deinit {
        print("good bye!")
    }
}

class Grade {
    let title: String
    var topStudent: Student?

    init(title: String) {
        self.title = title
    }

    deinit {
        print("good bye grade!")
    }
}

var leeo: Student? = Student(name: "leeo")
var grade: Grade? = Grade(title: "The Best")
print("leeo arc : \(CFGetRetainCount(leeo))") // 2
print("leeo arc : \(CFGetRetainCount(grade))") // 2

leeo?.grade = grade
grade?.topStudent = leeo

print("leeo arc : \(CFGetRetainCount(leeo))") // 3
print("leeo arc : \(CFGetRetainCount(grade))") // 3

leeo = nil
grade = nil

print("leeo arc : \(CFGetRetainCount(leeo))") // error -> leeo is nil
print("leeo arc : \(CFGetRetainCount(grade))")
```

위 처럼 서로 강한 참조를 하면 ARC는 1씩 늘어납니다. 그리고 leeo가 nil로 참조를 해제하더라도 `grade?.topStudent`가 `Student(name: "leeo")`를 참조하고 있어서 ARC는 2입니다. 하지만 더 이상 저 참조를 끊을 수 있는 방법이 없습니다. `grade?.topStudent = nil`도 할 수 없기 때문이죠.

이 로직에 계속 사용된다면, 더 이상 접근할 수도 없는 할당된 메모리가 늘어납니다. 말 그대로 메모리누수(memory leak) 가 일어나게 되는겁니다.

## 해결방법?

```
import Foundation
class Student {
    let name: String
    weak var grade: Grade?

    init(name: String) {
        self.name = name
    }

    deinit {
        print("good bye!")
    }
}

class Grade {
    let title: String
    weak var topStudent: Student?

    init(title: String) {
        self.title = title
    }

    deinit {
        print("good bye grade!")
    }
}

var leeo: Student? = Student(name: "leeo")
var grade: Grade? = Grade(title: "The Best")
print("leeo arc : \(CFGetRetainCount(leeo))") // 2
print("leeo arc : \(CFGetRetainCount(grade))") // 2

leeo?.grade = grade
grade?.topStudent = leeo

print("leeo arc : \(CFGetRetainCount(leeo))") // 2
print("leeo arc : \(CFGetRetainCount(grade))") // 2

leeo = nil  // good bye!
grade = nil // good bye grade!
```
이렇게 해결할 수 있습니다.

## 정리
평소에 잘 신경쓰지 않던 메모리 할당과, 해제, 누수에 관한 것들을 정리했습니다. 설게를 하거나 메모리 누수가 일어나는 부분을 약한참조로 바꾸고 로직이 변경되지 않도록 개발하는 방법을 공부해야겠네요!
