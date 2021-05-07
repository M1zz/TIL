파이어 베이스를 이용해서 데이터 저장하고 읽어오기

앱을 만들다보니, 데이터를 저장하고 저장된 데이터를 읽어서 보여주어야 하는 상황에 놓이게 되었습니다.  
처음에는 변수나 더미 데이터를 만들어서 저장하고 불러왔지만, 결국에는 서버를 셋팅 해야하나 라는 고민을하게 되었습니다.
그러다가 발견한 파이어 베이스의 기능 중 하나인 Database를 간단하게 사용한 방법에 대해 정리해 놓겠습니다.

### install 
파이어 베이스를 사용하기 위해서는 [패키지를 설치](https://github.com/firebase/firebase-ios-sdk/blob/master/SwiftPackageManager.md)해 줘야합니다.  
공식 문서를 보고 설치하면 어렵지 않습니다. 귀찮으신 분들을 위해 간략히 적어놓자면, Xcode의 SwiftPackageManager를 사용해 `https://github.com/firebase/firebase-ios-sdk.git`를 추가하면 됩니다.
그 중에서도 `FirebaseDatabase`를 꼭 설치해주세요.

Firebase에서 새 프로젝트를 만드신 후에 iOS프로젝트를 추가 해 줍니다. 설명을 따라 진행하면 어렵지 않게 완료할 수 있습니다.
핵심은 번들ID를 잘 입력 해주는 것과 `GoogleService-Info.plist`파일을 잘 다운받아 내 프로젝트에 넣어주는 것 입니다.

### Data 
firebaseDatabase는 JSON 형식의 데이터를 지원합니다. 간단히 JSON을 설명 드리면, 키 - 밸류 로 이루어진 데이터 포맷입니다.
우리는 간단한 JSON형식의 데이터를 작성해 볼것 입니다. 예제 데이터는 아래와 같이 생겼습니다.

```
{
  user {
    age : 31,
    married : false
    name : "hyunho"
  }
}
``` 

### Data Write 
데이터 베이스가 비어있으니 일단 쓰는 기능부터 구현해 보겠습니다.
```
import Firebase
let testItemsReference = Database.database().reference(withPath: "test-items")
let userItemRef = testItemsReference.child("user")
let values: [String: Any] = [ "age": 31, "married": false, "name": "hyunho"]
userItemRef.setValue(values)
```
코드가 동작하면, 최상위 root에서 child인 user를 찾아가고, 그 하위의 값을 키 - 밸류로 이루어진 값으로 설정할 수 있습니다.

### Data Read
전체 데이터를 읽어오는 코드는 아래와 같습니다 
```
testItemsReference.observe(.value, with: {
            snapshot in
            print(snapshot)
        })


testItemsReference.child("user").observe(.value) {
            snapshot in
            let value = snapshot.value as! [String: AnyObject]
            let name = value["name"] as! String

            print("name is \(name)")
       }
       // name is hyunho
```

### 정리
힘들게 API 서버를 셋팅할 필요 없이, 프로토 타입을 구현할 때는 Firebase의 기능을 이용해서 구현하면
앱 개발자 혼자서도 서비스를 꾸려나갈 수 있어 보였습니다.  
사용하면서 보였던 추가적인 문제들을 해결하면, 글 아래에 달아 놓던지 아니면 따로 글을 작성하도록 하겠습니다.








