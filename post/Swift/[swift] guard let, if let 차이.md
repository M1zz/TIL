Optional 변수를 안전사게 사용하기 위해 방법을 찾던 중 if let과 guard let에 대해 정리하겠습니다.

변수에 값이 올지 않올지 확신할 수 없을 때 Optional을 씁니다.
그러면 Optional 타입의 변수에 값은 어떻게 꺼내어 사용할 수 있을까요? 간단합니다 옵셔널 바인딩을 해서 사용합니다.
값이 올지 안올지 정확히 알 수 없을 때 [Optional 타입의 변수](https://dev200ok.blogspot.com/2020/03/swift-optional.html)를 사용하는데요 잘 모르는 분들이 이 전 글을 참고 해주세요!

안전하게 Optional 타입의 변수를 Unwrap하기 위해서는 옵셔널 바인딩을 해야하는데요 크게 두 두가지 방법이 있습니다.
바로 if let과 gurad let 입니다.

### 코드로 보는 예제
다음 예제 코드를 통해 둘의 차이점을 살펴 보겠습니다.
```
func fullName(name:String, rawPrefix:String?){
    
    // if let을 사용했을 경우
    if let prefix = rawPrefix {
        print(prefix+name)
    }
    else {
        print("need prefix")
    }

	// guard let을 사용했을 경우
    guard let prefix = rawPrefix else {
        print("need prefix")
        return   
    }
    
    print(prefix+name)
}

fullName(name: "hyunho", rawPrefix: "Lee")
```
풀네임을 작성할 때 이름은 필수이지만 성은 언제나 들어오지는 않는 옵션널 한 값 입니다.

guard let을 사용하는 경우에 이름만 들어오고 성이 들어오지 않았다면, 이런 케이스는 예외 케이스, 혹은 원하지 않는 상황 이라고 판단합니다. 그렇기 때문에 "need prefix"만 출력하고 함수를 종료합니다. 요구사항을 만족하지 못하는 경우를 정확히 보여줍니다. 마치 풀 네임을 만들 때 성이 없으면, 종료합니다.

하지만 if let 을 사용하면 성이 nil일 때와 nil이 아닐 때로 분기해서 코드를 작성하게 됩니다. 그렇기 때문에 사용자의 풀네임을 만들 때 성이 없으면 안돼는지, 없어도 되는건지 파악하려면 if문 안의 내용을 한번 더 읽어야 합니다. 더불어 코드도 길어지게 됩니다.

### 정리
정리하면 조건을 가지고 분기해서 처리하려면 if let을 사용해 줍니다. nil일 경우와 아닐 경우를 모두 작성 해주면 좋겠죠?  
반면에, 요구사항만을 반영해 예외상황을 처리하려면 gaurd let을 사용하면 좀 더 간결한 코드가 될 수 있습니다.
