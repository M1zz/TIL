# UIKit animation의 시작 포인트(작성중)

애니메이션은 시작위치, 끝위치, 시간 이 세가지를 지정하면 처음에서 끝위치로 지정한 시간 동안에 이동합니다. 앞으로 어떻게 애니메이션을 줄 수 있는지 정리해 보도록 하겠습니다.

## 오토레이아웃으로 애니메이션 주기

보통 뷰에는 제약(NSLayoutConstraint)이 걸려있습니다. 오토레이아웃으로 위치와 크기를 잡아주면서 걸어놓습니다. 오토레이아웃으로 x, y, width, height를 IBOutlet으로 연결 하듯이 제약을 만들어 애니메이션을 줄 수 있습니다.

먼저 버튼 클릭, viewLoad와 같이 언제 애니메이션이 시작될지 트리거를 지정해 둡니다. 그리
`@IBOutlet weak var menuHeightConstraint: NSLayoutConstraint!`와 같이 내가 만든 제약을 하나 추가해줍니다. 이곳이 시작지점이 되겠죠? 그리고 종료 지점을 정의 해 줍니다.
```
UIView.animate(withDuration: 1) {
        self.menuHeightConstraint.constant = self.menuIsOpen ? 200 : 80
        self.view.layoutIfNeeded()
    }
```
`layoutIfNeeded()`를 호출해줍니다. 그러면 view가 업데이트 되면서 높이가 변하는게 애니메이션 처럼 보입니다.

```
UIView.animate(
      withDuration: 1, delay: 0,
      options: .curveEaseIn,
      animations: {
        self.menuButton.transform = .init(rotationAngle: self.menuIsOpen ? .pi / 4 : 0)
        self.view.layoutIfNeeded()
      }
    )
```
또한 transform 를 활용하면 회전에 대한 애니메이션을 줄 수 있습니다. 애니메이션의 가속(?) 옵션으로는
- curveEaseIn : 천천히 출발해서 가속도
- curveEaseOut : 가속도를 내서 출발해 천천히
- curveEaseInOut : 중간에 빨라졌다 천천히
와 같은 옵션이 있으니 확인 해두시면 좋을 것 같습니다
