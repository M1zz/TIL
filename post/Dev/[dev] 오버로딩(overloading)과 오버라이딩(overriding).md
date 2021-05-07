오버로딩과 오버라이딩

오버로딩과 오버라이딩에 대해 정리해 놓는다.

### 오버로딩(overloading)
오버로딩이란 같은 이름의 메소드에 매개변수는 다르게 선언 할 수 있는 것을 의미한다.

```
class Student {
    var name: String
    var age: Int?
    
    init(name: String, age: Int?) {
        self.name = name
        self.age = age
    }

    func printInfo(){
        print("my name is \(name)")
    }
    
    func printInfo(age: Int){
        print("my name is \(name) my age is \(age)")
    }
}
var hyunho = Student(name: "hyunho", age: nil)
var jihye = Student(name: "jihye", age: 1)
hyunho.printInfo()
jihye.printInfo(age: jihye.age!)

// my name is hyunho
// my name is jihye my age is 17
```
위의 코드에서 이름은 필수지만 나이는 옵션이다. 이 때 자기소개 출력 함수의 경우 이름만 있을 경우와 이름과 나이가 있는경우에 다른 결과가 출력 되어야 하기 때문에 오버로딩을 통해 다른 결과를 출력했다.

### 오버라이딩(overriding)
상위 클래스에서 선언한 메서드를 하위 클래스에서 재정의해서 사용하는 것.
```
class Student {
    var name: String
    var age: Int?
    
    init(name: String, age: Int?) {
        self.name = name
        self.age = age
    }

    func printInfo(){
        print("my name is \(name)")
    }
    
    func printInfo(age: Int){
        print("my name is \(name) my age is \(age)")
    }
}

class MiddleSchoolStudent: Student{
    override func printInfo(age: Int) {
        print("my name is \(name) and I'm middleschool student")
    }
}
var hyunho = Student(name: "hyunho", age: nil)
var jihye = MiddleSchoolStudent(name: "jihye", age: 18)
hyunho.printInfo()
jihye.printInfo(age: jihye.age!)
```
`override`라는 키워드를 메서드 앞에 붙여 사용한다. 매개변수의 이름, 타입, 리턴타입까지 모두 맞춰 주어야 한다.


