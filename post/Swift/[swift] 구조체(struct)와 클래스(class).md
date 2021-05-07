구조체와 클래스는 코드블럭을 만들 때 쓰이고 그 문법 또한 매우 닮았다. 하지만 다른점이 있기 때문에 그 특성을 잘 파악하고, 필요한 부분에서 활용할 수 있도록 정리한다.  

### 정의 방법 (Definition)
구조체는 `struct`, 클래스는 `class`라는 키워드를 사용하여 정의한다.  

```
struct Student{
// properties and methods
}

class Student {
// properties and methods
}
```

### 프로퍼티와 메소드 (Properties and Methods)
이 예제에서는 학생의 구조체와 클래스의 정의 예제이다. SomeStudent struct 와 AnotherStudent class 모두 firstName, lastName, grade라는 properties를 가지고 grade를 출력해주는 printGrade() 메소드를 가진다.  
다른 점은 클래스에서는 init() 함수를 따로 정의 해줘야 한다.
```
struct SomeStudent{
  // properties
  let firstName: String
  let lastName: String
  var grade: Int

  // methods
  func printGrade(){
    print("grade is \(grade)")
  }
}

class AnotherStudent {
  // properties
  let firstName: String
  let lastName: String
  var grade: Int

  // methods
  init(firstName: String, lastName: String, grade: Int){
    self.firstName = firstName
    self.lastName = lastName
    self.grade = grade
  }

  func printGrade(){
    print("grade is \(grade)")
  }
}
```

### 구조체와 클래스의 인스턴스 생성 (struct and class Instance)
class와 struct는 옵셔널이 아닌 프로퍼티 값을 채워주면서 인스턴스를 생성한다.  
class의 경우에도 따로 init()을 호출하지 않는다. 호출 된 인스턴스들은 초기화 되면서 생성된다.
```
let sam = SomeStudent(firstName: "sam", lastName: "smith", grade: 4)
let john = AnotherStudent(firstName: "john", lastName: "lennon", grade: 5)
```

### 속성 접근하기(Accessing Properties)
생성된 인스턴스에 대해 .(dot)을 이용하여 각각의 속성에 접근할 수 있다.  
```
print(sam)
print(sam.firstName)
print(sam.lastName)
print(sam.grade)
sam.printGrade()
/*
SomeStudent(firstName: "sam", lastName: "smith", grade: 4)
sam
smith
4
grade is 4
*/

print(john)
print(john.firstName)
print(john.lastName)
print(john.grade)
john.printGrade()
/*
__lldb_expr_19.AnotherStudent
john
lennon
5
grade is 5
*/
```

### 레퍼런스와 값(Reference and Value)
위의 예제에서 보이듯이, struct와 class를 호출 했을 때 다른 결과값을 보인다.  
struct의 인스턴스는 값 타입을 가진다. 하지만 class의 인스턴스는 참조타입이다.  

아래의 예시를 살펴보자. struct로 생성된 sam 인스턴스를 dan이라는 변수에 대입한 뒤 dan에 grade를 변경하면 sam과 dan이 각각 다른 값을 가진다.  
sam에 있는 값을 dan에 대입하고, dan에 grade에 다른 값을 대입했을 때 dan과 sam이 다른 grade값을 가진다.
```
var dan = sam
dan.grade = 6
print(dan.grade)
print(sam.grade)
/*
6
4
*/
```
클래스의 경우 john과 lina는 값은 객체를 참조하고(바라고보) 있기 때문에 lina의 grade를 바꿨음에도 john의 grade까지 바뀐 것을 볼 수 있다.
```
var lina = john
lina.grade = 8
print(lina.grade)
print(john.grade)
/*
8
8
*/
```

### 클래스와 구조체의 선택하기(Choosing Between Classes and Structures)
참고링크 : https://developer.apple.com/documentation/swift/choosing_between_structures_and_classes

- 기본적으로 struct를 사용하세요.
 - 일반적인 종류의 데이터를 나타내는 구조를 사용하십시오
- Objective-C 상호 운용성이 필요할 때 class를 사용하세요.
 - 데이터를 처리해야하는 Objective-C API를 사용하거나 Objective-C 프레임 워크에 정의 된 기존 클래스 계층 구조에 데이터 모델을 적합시켜야하는 경우.
- 모델링 할 데이터의 ID를 제어해야 할 때 class를 사용하세요.
 - 두 개의 서로 다른 클래스 인스턴스가 각각의 저장된 속성에 대해 동일한 값을 가질 때 ID 연산자 (===)에 따라 여전히 다른 것으로 간주
 - 아이덴티티를 갖기 위해 인스턴스가 필요할 때 클래스를 사용
- struct와 프로토콜을 사용하여 모델상속 및 동작을 공유하세요.
 - 처음부터 상속 관계를 구축하는 경우 프로토콜 상속을 선호하십시오.
