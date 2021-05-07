Xcode에서 `Option + Left-click` 을 통해 프로퍼티나 메소드에 대한 정보를 얻을 수 있다. 이 정보를 quick help라고 한다.  

아래와 같은 코드가 있을 때 quick help를 통해 어떤 정보를 얻을 수 있는지 살펴보자.
```
struct Car {
    let speed: Int
    let modelName: String
    
    func stopCar() -> Bool{
        return true
    }
}

let benz = Car(speed: 30, modelName: "benz")
benz.stopCar()
```
메소드에서 quick help를 사용하면 선언된 간략한 정보들을 얻을 수 있다. 
프로퍼티의 경우에는 선언 부분을 볼 수 있고, 메소드의 경우에는 매개변수와 반환 값 등을 볼 수 있다. 

> Declaration : 프로퍼티나 메소드의 정의 부분을 보여준다.  
> Parameters : 매개변수의 정보를 보여준다.  
> DeclaredIn : 어느 파일에 선언되어 있는지 보여준다.  


사용자가 주석 블록을 추가함으로써 quick help에 더 많은 정보를 넣을 수 있다.
예를 들면 Parameters의 
주석의 블록은 `/** */` 기호 사이에 설명을 넣을 수 있다.
다음과 같은 형태를 가지며, 스트럭트의 경 요약과 설명을 메소드의 경우 매개변수와 에러핸들링 반환 값에 대한 설명을 추가할 수 있다.
```
/**
  Summary에 해당하는 내용

  Discussion에 해당하는 내용

  - parameters:
    - grade: 학년

  - Throws: 여러 에러가 발생할 수 있습니다.
 
  - returns: 학년을 반환합니다.

*/
```

아래의 코드에서 grade가 학생의 점수인지, 학년인지 모호할 수 있기 때문에 주석으로 설명을 넣어주면 좀 더 명확해진다.  
물론 이름을 잘 짓는게 가장 선호되는 방법이다.

```
/**
 학생을 정의 한 struct입니다.
    
 학생의 이름, 주소, 학년을 입력해주세요.
 */
struct Student {
    let name: String
    let address: String
    var grade: Int
    
    /**
        메소드에 대한 요약을 적습니다.
            
        그 다음 줄은 무조건 설명입니다.
            
        - parameters:
            - name: 이름
     */
    func printName(name: String){
        print("내 이름은 \(name) 입니다.")
    }
    
    /**
        - Throws: 숫자만 와야 합니다.

        - returns:
            - 학년을 반환합니다.
     */
    func getGrade() throws -> Int{
        return self.grade
    }
    
}
```

참고링크 : https://developer.apple.com/library/archive/documentation/Xcode/Reference/xcode_markup_formatting_ref/SymbolDocumentation.html


