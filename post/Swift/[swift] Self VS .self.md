코딩을 하다 보니, Self.name 을 쓸 때고 있고, leeo.self도 있어서 self라는 키워드는 언제 어떻게 써야하는지에 대해 정리해 놓겠습니다.


### Self
`Self`라는 [키워드](https://github.com/apple/swift-evolution/blob/master/proposals/0068-universal-self.md)는 자기 자신의 동적 클래스를 가리킵니다. 
아래의 코드를 보시면 Self는 클래스 자기 자신을 가리켜 Self. 으로 인스턴스의 프로퍼티나 메소드에 접근할 수 있는 것을 볼 수 있습니다.

```
class Student {
    class var name: String {
        return "Leeo"
    }
    
    func printName() {
        print(Self.name)
    }
}

class Leeo: Student {
    override class var name: String {
            return "hyunho"
        }
    
}
let student = Student()
student.printName()


let leeo = Leeo()
leeo.printName()
```

### .self 
`.self`를 이해하려면 Metatype에 대해서 알아야 합니다. 이 글의 주제는 아니기 때문에 .self를 언제 사용하는지에 대해서만 간단히 정리하겠습니다.  
아래의 결과가 모두 이해된다면 당신은 이미 .self에 대하여 알고 있는 것 입니다! 아니라면 Metatype에 대해서도 공부해 보시길 권해드립니다.  
```
let word = "testString"
print(word is String)       // true
print(word is String.Type)  // false
print(type(of: word) is String.Type)  // true

print(type(of: word) == String.self)  // true
//print(type(of: word) is String.self)  // error
//print(type(of: word) == String.Type)       // error
```

첫 번째 word 라는 변수에 String Type의 문자열을 대입했습니다. 즉 word는 String Type입니다.  
그렇기 때분에 타입을 비교하는 첫번째 `word is String`는 참 입니다. 하지만 `String.Type`이 "testString"은 아닙니다. 
그렇기 때문에 두 번째 예제는 false입니다.
세 번째 `type(of: word) is String.Type` String의 타입은 word의 타입과 같은 타입입니다.

자 그럼 .self는 언제 사용될까요? 타입을 비교하는 is가 아닌 값을 비교하는 경우에 사용됩니다.
String타입의 값은 `type(of: word) == String.self`값과 같습니다. 하지만 .self를 사용해서 값으로 만들어 버린 다음에는
타입 비료글 할 수 없습니다. 마찬가지로 타입을 == 연산자로 비교할 수 없습니다. 

### 정리
차이점만 살펴보고 나니 Self와 .self는 엄청난 차이가 있었습니다. 이제 각각의 Self 키워드를 언제 어떻게 적절하게 사용해야 할지 공부 해 봐야겠습니다.








