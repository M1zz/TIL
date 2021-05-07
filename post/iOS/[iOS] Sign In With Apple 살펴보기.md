### Sign in with Apple

애플 아이디로 로그인을 할 수 있는 Sign in with Apple이 공개 된 후 적용하고 이해한 내용을 기록으로 남겨둔다.

참고 URL : https://developer.apple.com/documentation/authenticationservices/adding_the_sign_in_with_apple_flow_to_your_app

순서 : https://developer.apple.com/documentation/sign_in_with_apple/sign_in_with_apple_rest_api/authenticating_users_with_sign_in_with_apple#see-also

### 버튼 추가
1. 새 프로젝트를 만들고 ViewController에 버튼을 호출하기 위한 함수를 추가한다.
2. import AuthenticationServices 해준다.
3. @IBOutlet weak var loginProviderStackView: UIStackView! 프로퍼티를 추가한다.

```
func setupProviderLoginView() {
    let authorizationButton = ASAuthorizationAppleIDButton()
    authorizationButton.addTarget(self, action: #selector(handleAuthorizationAppleIDButtonPress), for: .touchUpInside)
    self.loginProviderStackView.addArrangedSubview(authorizationButton)
}

@objc
func handleAuthorizationAppleIDButtonPress() {

}
```


### 인증 버튼 동작
```
extension ViewController: ASAuthorizationControllerDelegate {
    
}
extension ViewController: ASAuthorizationControllerPresentationContextProviding {
    func presentationAnchor(for controller: ASAuthorizationController) -> ASPresentationAnchor {
        <#code#>
    }
    
    
}
```
ASAuthorizationControllerDelegate,
ASAuthorizationControllerPresentationContextProviding
이 두개의 프로토콜을 확장한다.

```
@objc
func handleAuthorizationAppleIDButtonPress() {
    let appleIDProvider = ASAuthorizationAppleIDProvider()
    let request = appleIDProvider.createRequest()
    request.requestedScopes = [.fullName, .email]
    
    let authorizationController = ASAuthorizationController(authorizationRequests: [request])
    authorizationController.delegate = self
    authorizationController.presentationContextProvider = self
    authorizationController.performRequests()
}
```
버튼이 눌렸을 때 불리는 handleAuthorizationAppleIDButtonPress 함수를 구현해준다.
하지만 실제 Sign in with Apple으로 로그인을 하려면 기능을 추가해야한다. 

> Targets > Signing & Capabilities > + Capability > Sign in with Apple  

어플리케이션에 Sign in with Apple 을 추가해준다.

`request.requestedScopes = [.fullName, .email]` 으로 요구할 사용자의 데이터 범위를 표기한다.


 ### 인증 결과

 인증 결과를 제공하기 위한 인터페이스인 `ASAuthorizationControllerDelegate`를 살펴보자

 링크 : https://developer.apple.com/documentation/authenticationservices/asauthorizationcontrollerdelegate

> 성공 : optional func authorizationController(controller: ASAuthorizationController, didCompleteWithAuthorization authorization: ASAuthorization)

> 실패 : optional func authorizationController(controller: ASAuthorizationController, didCompleteWithError error: Error)

```
extension ViewController: ASAuthorizationControllerDelegate {
    
    func authorizationController(controller: ASAuthorizationController, didCompleteWithAuthorization authorization: ASAuthorization){
        switch authorization.credential {
        case let appleIDCredential as ASAuthorizationAppleIDCredential:
            // appleIDCredential로 로그인
            if let _ = appleIdCredential.email, let _ = appleIdCredential.fullName {
                registerNewAccount(credential: appleIdCredential)
            }
            else {
                signInWithExistingAccount(credential: appleIdCredential)
            }
            break
            
        case let passwordCredential as ASPasswordCredential:
            // passwordCredential로 로그인
            break
        default:
            break
        }
    }

    func authorizationController(controller: ASAuthorizationController, didCompleteWithError error: Error){
          // 에러 처리 
    }
    
}
```
appleIDCredential로그인 했을 때 처음로그인 하는 것이라면 사용자의 정보를 저장 해준다. 이미 기존에 로그인을 했었던 회원이라면, 기존의 정보로 진행한다. 

```
private func registerNewAccount(credential: ASAuthorizationAppleIDCredential) {
        showTokenInfo(credential: credential)
        // keyChain에 사용자 정보 저장
        // appleId 사용 자동 등록
        // 로그인 페이지로 이동
}
    
private func signInWithExistingAccount(credential: ASAuthorizationAppleIDCredential) {
        showTokenInfo(credential: credential)
        // 이미 있는 정보로 로그인 
}
```

https://developer.apple.com/documentation/authenticationservices/asauthorizationappleidcredential

### 인증서 Revoke 시키기
한번 등록된다면 그 다음부터는 email이 전송되지 않는다. 그러므로 다시 등록하려면 Apple ID 사용 앱 리스트에서 제거 해줘야한다.
> 아이폰 설정 > 사용자 (맨위) > 암호 및 보안 > 내 Apple ID를 사용하는 앱 > 앱 > Apple ID 사용 중단








