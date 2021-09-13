
instance vs class vs static

클래스를 사용할 때에 static이 붙은 메소드를 사용하지 못해서 문득 아 얘를 언제 쓰는지 모르고 있구나 라는 생각에 정리해 두겠습니다.  
func 라는 키워드는 함수를 만들 때 많이 들 보셨을 것이라고 생각합니다. 오늘은 이 앞에 붙는 키워드들의 종류와 그 이유에 대해 알아보고 정리하겠습니다.

- 아무것도 붙지 않음
  - func instanceFunc() {}
- class가 붙음
  - class func classFunc() {}
- static이 붙음
  - static func staticFunc() {}


### How to make Func 
아무것도 붙지 않은 함수는 평범하게 아래와 같이 사용이 가능합니다.
```
class Student {
    func instanceFunc(){
        print("instance func leeo")
    }
    class func classFunc() {
        print("class func leeo")
    }
    static func staticFunc() {
        print("static func leeo")
    }
}

let student = Student()
student.instanceFunc()

Student.classFunc()
Student.staticFunc()
// instance func from Parents
// class func from Parents
// static func from Parents
```

그렇다면 다른 두 키워드 `class`, `static`은 언제 쓰는 것이고 어떤 장점이 있는 걸까요?  
이 두 키워드는 클래스가 만든 인스턴스에서 사용하는것이 아닌 클래스 자체에서 사용하는 메소드입니다.
이 두개를 타입 메소드라고 부르는데 어떤 의미냐 하면, 인스턴스는 각각의 인스턴스에 따라 다른 값들이 반환될 수 있는 반면에
타입 메소드는 이 타입이라면 반드시 가지고 있어야 할 것들입니다.  

학생의 타입에서 필요한 메소드 들은 각각의 인스턴스에 구현하는 것이 아니라 클레스에 타입 메소드로 구현해야 할 것입니다. 예를들면 모든 학생들의 기상시간을 알려주는 메소드를 구현한다면, 학생이라는 타입에
종속 적이기에 인스턴스가 아닌 타입 메소드로 구현해주면, 공통 부분이라는 것이 명시적으로 표현될 것 같습니다.  

### static VS class 
그렇다면 타입 메소드를 만들어 주는 이 두 키워드는 어떤 차이점이 있을까요?

```
class Leeo: Student {
    func printName(){
        print("My name is Leeo")
    }
    override class func classFunc() {
        print("Child class func leeo")
    }
}
let leeo = Leeo()
leeo.printName()
leeo.instanceFunc()
Leeo.classFunc()
Leeo.staticFunc()
// My name is Leeo
// instance func from Parents
// Child class func leeo
// static func from Parents
```
`class` 키워드가 붙은 메소드는 override가 가능합니다. 하지만 `static` 키워드가 붙은 메소드는 상속이 붙지 않습니다.
물론 `final` 이라는 키워드를 class 앞에 붙이면 상속이 불가능하니, 취향에 따라서 아래 중 하나를 선택하시면 됩니다.

```
class -> static  
class -> final class
```

### 정리
- 인스턴스용 메소드
  - func
- 클래스용 메소드
  - class - override 가능 
  - static - override 불가능 










