어떤 버튼을 눌러 다른 페이지로 이동 시키는 일은 항상 헷갈립니다.  
이번 기회에 정리해 두고 가려고 합니다.

일단 버튼을 하나 만들고 눌러서 내가 원하는 페에지와 실제 페이지가 잘 나타나는지 정리하겠습니다.

이런 버튼이 하나 있고 화면을 전환 해 보도록 하겠습니다.

### 스토리 보드
가장 쉬운 방법이라고 생각합니다. 내가 액션을 줄 컴포넌트를 클릭하고 컨트롤 + 드래그로 원하는 페이지에 드롭합니다.  
선이 생기면서 어떻게 보여줄 것인지 segue action을 선택하면 됩니다. 일단은 present modally를 선택합니다.  
버튼을 누르면 모달 창이 새로 생기죠? 아주 쉽고 간편하답니다!

### viewController present
```
@IBAction func buttonClick(_ sender: Any) {
    let viewControllerName = self.storyboard?.instantiateViewController(withIdentifier: "SecondViewID")
    if let view = viewControllerName {
        self.present(view, animated: true, completion: nil)
    }    
}
```
다음과 같은 코드를 작성해줍니다. 스토리 보드에 만들어 준, 뷰컨트롤러의 스토리보드 아이디를 설정 해줍니다. 저는 `SecondViewID`로 설정 해주었습니다.
버튼에 IBAction을 설정 해 준다음에 스토리 보드에서 스토리 보드에서 설정했던 ID를 가지고 옵니다.
그리고 가져온 뷰컨트롤러를 self에서 present 해줍니다.
viewControllerName?.modalTransitionStyle를 설정하여 모달의 전환을 설정할 수 있습니다. 

```
@IBAction func buttonClick(_ sender: Any) {
    let viewControllerName = self.storyboard?.instantiateViewController(withIdentifier: "SecondViewID")
    viewControllerName?.modalTransitionStyle = .flipHorizontal
    if let view = viewControllerName {
        self.present(view, animated: true, completion: nil)
    }
}
```
또한 view.modalPresentationStyle = .fullScreen 을 추가해서 화면 가득 채울 수 있습니다.
```
@IBAction func buttonClick(_ sender: Any) {
    let viewControllerName = self.storyboard?.instantiateViewController(withIdentifier: "SecondViewID")
    viewControllerName?.modalTransitionStyle = .flipHorizontal
    if let view = viewControllerName {
        view.modalPresentationStyle = .fullScreen
        self.present(view, animated: true, completion: nil)
    }
    
}
```

### NavigationController
present를 하면 뒤로 돌아갈 방법이 없습니다. 물론 present modally하게 되면 dismiss 할 수 있지만 fullscreen을 띄웠을 때는
뒤로 돌아갈 방법이 없습니다. 뒤에 뷰가 없어졌기 때문입니다.

NavigationController를 이용해서 스택에 넣어주고 뒤로가는 화면 전환에 대해 알아보겠습니다.

기존의 뷰에 네비게이션 컨트롤러를 Embed in view controller 해 주면 아래와 같이 뷰 컨트롤러 앞에 네비게이션 컨트롤러가 생기는 것을 볼 수 있습니다.
기존과 다른점은 네비게이션 스택에 쌓이기(push) 때문에 뒤로가기를 할 수 있습니다.

코드로 작성하려면 기존에 present 부분을 push 하도록 수정해주면 됩니다.

```
@IBAction func buttonClick(_ sender: Any) {
    let viewControllerName = self.storyboard?.instantiateViewController(withIdentifier: "SecondViewID")

    viewControllerName?.modalTransitionStyle = .flipHorizontal
    if let view = viewControllerName {
        self.navigationController?.pushViewController(view, animated: true)
    }
    
}
```

### 정리

화면 전환하는 방법에 대해서 정리 해 보았습니다. 스토리보드를 쓰는 방법도 좋고 필요하다면 뷰컨트롤러를 따로 그려서 필요한 순간에 코드로 불러오는 것도 좋을 것 같습니다.




