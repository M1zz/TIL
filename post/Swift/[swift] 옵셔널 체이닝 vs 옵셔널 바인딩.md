옵셔널 체이닝과 옵셔널 바인딩이 헷갈려 정리해 놓았습니다.

### 언제 필요할까요?

옵셔널은 프로퍼티에 값이 있을 수도있고, 없을 수도 없음을 나타내는데 사용되었습니다.  
옵셔널은 값이 없을 때 nil을 할당 해줍니다. 
문제는 우리가 필요한 값이 nil인지 아닌지 확인을 해야하는 번거로움이 있다는 것 입니다.

코드로 예를 들어보겠습니다. 

```
class Person {
    var name: String
    var job: String?
    var home: Apartment?
    
    init(name: String, home: Apartment?) {
        self.name = name
        self.home = home
    }
}

class Apartment {
    var buildingNumber: String
    var roomNumber: String
    var security: Person?
    var owner: Person?
    
    init(dong: String, ho: String, security: Person?) {
        self.buildingNumber = dong
        self.roomNumber = ho
        self.security = security
    }
}

let chulsoo: Person? = Person(name: "chulsoo", home: apart)
let apart: Apartment = Apartment(dong: "101", ho: "202", security: chulsoo)
let hyunho: Person? = Person(name: "leeo", home: apart)
```
leeo 라는 사람의 apart 에 있는 security의 이름이 알고 싶습니다.

```
print(leeo.home.security.name)
```

이렇게 출력하면 될 것 같습니다. 
하지만 security가 nil일 수 있으니, nil 검사를 해야 합니다. 

이런식으로 말이죠.
```
if let a = hyunho {
    if let b = a.home {
        if let c = b.security {
            print(c.name)
        }
    }
}
```

앞에 있는 요소들에 대해 모두 nil 검사를 하지 않아도 최종적으로 원하는 값에 접근 할 수 있는 방법이 바로 옵셔널 체이닝 입니다.
옵셔널 체인을 만들고 싶은 옵셔널 값 뒤에 물음표 '?'를 붙이면 체인이 완성됩니다.

체인은 하나의 옵셔널 값이기 때문에 결과값이 옵셔널로 떨어집니다. 그렇기 때문에 중간에 껴있는 옵셔널 값에 nil이 있어도 우리가 최종 적으로 접근하려는 값이 nil 임을 알 수 있죠.
```
print(leeo?.home?.security?.name) // Optional("chulsoo")
```

### ! 를 쓰는 것과는 무엇이 다른가요?

print(leeo?.home?.security?.name) // optional

print(leeo!.home!.security!.name) // non optional

이 둘은 큰 차이가 없어 보입니다. 왜냐하면 둘 다 잘 돌아가니까요!
차이는 바로 실패에서 알 수 있습니다. 실수를 관대하게 품어 줄 수 있는 sexy한 방법이죠

만약에 security가 nil 이라면, 물론 언제든지 security은 nil이 될 수 있습니다. Optional이니까요!

```
leeo!.home!.security = nil
print(leeo?.home?.security?.name) // nil
print(leeo!.home!.security!.name) // error
```

컴파일 할 때 에러도 안 내놓고 크래쉬를 내다니 배신감이 느껴집니다.

옵셔널 체이닝은 옵셔널 바인딩과는 다릅니다.  
저는 옵셔널 체이닝과 바인딩이 헷갈렸습니다. 이름 때문일까요... 라임?

짧게 정리하면 옵셔널 바인딩은 옵셔널 값 안에 실제 값이 있으면 꺼내오고 없으면 nil을 반환합니다.
하지만 옵셔널 체이닝은 그 결과가 `optional`값 입니다. 

그러므로 옵셔널 체이닝으로 옵셔널 값의 nil체크를 하고도 내가 필요한 값은 아직 Optional에 감싸져 있는 상태일테니
```
if let name = leeo?.home?.security?.name {
    print(name)
}
```
와 같이 바인딩을 해서 값을 꺼내주어야 합니다.


### Optional에 구현되어 있는 map을 이용하면 체이닝 처럼 nil값을 검사할 수 있다던데?
코드를 바로 보겠습니다.
```
let age: Int? = 30

if let myAge = age.map( {"She is \($0 + 10) years old"} ) {
    print(myAge)
}
```
옵셔널로 래핑된 값은 nil값을 검사하지 않고, 체이닝도 이용하지 않고 map을 사용하면 그 안의 값을 이용하거나, 형태를 변환할 수 있습니다.  
실제 값을 옵셔널 바인딩 하지 않고 삼항 연산자와 같은 연산자로 꺼내는 방법도 있습니다.
```
let newAge2 = age2 != nil ? age2! + 10 : nil
print("She is \(newAge2! + 10) years old")
```

flatMap, compactMap 들은 다음에...


