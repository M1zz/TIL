민감한 데이터를 저장하고, 저장된 데이터를 안전하게 꺼내어 사용자 인증 하는 과정을 Keychain을 이용하여 구현하는 방법을 소개합니다.

해당 포스트의 원문은 다음과 같습니다 : https://www.raywenderlich.com/129-basic-ios-security-keychain-and-hashing
소스코드 [다운로드](https://koenig-media.raywenderlich.com/uploads/2018/02/Friendvatars-1.zip)


#### AuthViewController.signInButtonPressed()
- 로그인 버튼이 눌렸을 떄 호출
- AuthViewController.signIn() 함수 호출

#### AuthViewController.signIn()
- 사용자의 입력 종료
- 이메일, 비밀번호 입력 받은 데이터 검증
- 디바이스의 이름 가져오기
- 이름, 이메일, 비밀번호로 User 데이터 만들기
- AuthController.signIn() 로그인 함수 호출

#### AuthController.signIn()
```
class func passwordHash(from email: String, password: String) -> String {
    let salt = "x4vV8bGgqqmQwgCoyXFQj+(o.nUNQhVP7ND"
    return "\(password).\(email).\(salt)".sha256()
  }
```
- 입력받은 이메일과 비밀번호를 넘겨줘 AuthController.passwordHash()로 해싱
- 키체인(Keychain)에서 앱의 데이터를 식별하는데 사용되는 서비스 이름을 상단에 정의
- KeychainPasswordItem.savePassword()로 암호 키체인에 저장 
- UserDefaults에 현재 유저정보 저장

#### KeychainPasswordItem(service: serviceName, account: user.email).savePassword(finalHash)
- 서비스 이름과 입력받은 이메일 그리고 해쉬값을 가지고 기존에 저장 되어있는 암호인지 검사 readPassword()
  - KeychainPasswordItem.keychainQuery()를 통해 쿼리생성
  - 쿼리를 실행해 비밀번호를 암호화 된 상태로 가져옴
  - 비밀번호가 있으면 입력받은 비밀번호의 암호화된 값를 가져옴
- 새로 입력받은 비밀번호로 키체인 비밀번호 업데이트
- 비밀번호가 없으면 새로운 아이템을 서비스 이름과 이메일 가지고 생성
- 새로운 아이템에 비밀번호 저장

#### AuthController.isSignedIn
```
static var isSignedIn: Bool {
    // 1
    guard let currentUser = Settings.currentUser else {
      return false
    }
    
    do {
      // 2
      let password = try KeychainPasswordItem(service: serviceName, account: currentUser.email).readPassword()
      return password.count > 0
    } catch {
      return false
    }
  }
```
위의 코드를 추가하여 앱 컨트롤러에서 인증된 사용자인지 확인한다.

#### AppController.handleAuthState()
```  
@objc func handleAuthState() {
    if AuthController.isSignedIn {
      rootViewController = NavigationController(rootViewController: FriendsViewController())
    } else {
      rootViewController = AuthViewController()
    }
  }
  ```

이제 handleAuthState()가 제대로 동작한다. 하지만 로그인 후 UI를 업데이트 해주기 위해서는 재시작을 해야한다. 이 방법은 불편하기 때문에, 인증과 같은 상태가 변경된 것을 앱에게 알려주는 좋은 방법은 알림(notifications)사용이다.

#### AuthController.signIn()
- NotificationCenter.default.post(name: .loginStatusChanged, object: nil) 추

#### AppController.init()

```
init() {
    NotificationCenter.default.addObserver(
      self,
      selector: #selector(handleAuthState),
      name: .loginStatusChanged,
      object: nil
    )
  }
```

- AppController가 로그인 알림 옵져버로 등록. 알림이 발생할때 handleAuthState가 호출

#### AuthController.signOut()
- 현재 유저정보를 확인
- KeychainPasswordItem(service: serviceName, account: currentUser.email).deleteItem() 키체인에서 삭

#### FriendsViewController.signOut()
- signOut기능 추가

```
@objc private func signOut() {
    // sign out
    try? AuthController.signOut()
  }
```
