delegate를 구현하다가, delegate에 대해 정리 해보는 기회를 가지면 좋을 것 같아서 좋은 예제를 보고 정리해놓는다.  

### 정의  
delegate를 사전에 검색해 보면 대리자, 위임자라고 나온다.  delegate를 쓰면 무언가를 위임 받는 것 같다.  
실제 사용되는 곳을 보면 프로토콜을 사용해서 무엇을 구현해야 하는지 제안한다.  

### 간단한 예제
이름을 입력받아, 라벨에 출력해주는 앱을 제작한다.
구현방법은 다음과 같다. 

```
import UIKit

class ViewController: UIViewController {

    @IBOutlet weak var nameLabel: UILabel!
    @IBOutlet weak var textField: UITextField!
    
    override func viewDidLoad() {
        super.viewDidLoad()
    }

    @IBAction func buttonClicked(_ sender: Any) {
        nameLabel.text = textField.text;
    }
}
```
버튼이 눌릴 때, `nameLabel.text = textField.text;` 하게 된다. 
비슷한 기능을 delegate를 사용해서 구현해보자.

### UITextFieldDelegate 
TextField에서 일어나는 일에 대한 위임을 ViewController가 받아서 처리해보자. 일단 자 이제부터 이녀석 에게 일어나는 일은 내가 처리해줄게! 를 선언합니다. 
```
override func viewDidLoad() {
    super.viewDidLoad()
    textField.delegate = self # self는 ViewController입니다
}
```
간단하죠? textField의 위임자(delegate)는 self야 가 굉장히 직관적입니다.
그럼 그 다음에 textField에게는 어떤일이 일어날 수 있는지 봅시다. 
하지만 그 전에 `Cannot assign value of type 'ViewController' to type 'UITextFieldDelegate?'`라는 에러가 발생합니다.  
그 이유는 textField.delegate는 프로토콜인데 우리는 UITextFieldDelegate 프로토콜을 채택하지 않았기 때문이죠. 그러면 채택 해 볼까요 `Fix`를 누릅니다.  
> class ViewController: UIViewController, UITextFieldDelegate   

위와 같이 컨트롤러에 , 로 구분을 해 채택 해 줍니다.

다시 돌아와서 textField를 맡게 되었으니! 우리가 무슨일을 할 수 있는지 알아봅시다. 커맨드 키를 누른 채로 UITextFieldDelegate를 눌러 Jump to definition으로 가봅시다. 
```
@available(iOS 2.0, *)
optional func textFieldShouldClear(_ textField: UITextField) -> Bool // called when clear button pressed. return NO to ignore (no notifications)

@available(iOS 2.0, *)
optional func textFieldShouldReturn(_ textField: UITextField) -> Bool // called when 'return' key pressed. return NO to ignore.
```
뭔가 엄청 많네요. 일부분만 가져와 봅시다.  일단 눈으로 슥 봤을 때 다 optional인 것을 보니 꼭 구현해야 하는 건 없어보이네요. 그러면 입맛대로 구현해봅시다. 
가장 아래의 `textFieldShouldReturn`를 구현해 봅시다. 
설명을 읽어보니 'return'키(엔터키)가 눌렸을 때 불리는 녀석이랍니다. 반환값은 Bool인데 false를 반환하면, 눌리는 것을 무시합니다. true이면 'return'키(엔터키)가 눌렸을 때, 호출되나봅니다. 구현해봅시다 야호!

### textFieldShouldReturn
```
import UIKit

class ViewController: UIViewController, UITextFieldDelegate {

    @IBOutlet weak var nameLabel: UILabel!
    @IBOutlet weak var textField: UITextField!
    
    override func viewDidLoad() {
        super.viewDidLoad()
        textField.delegate = self
        
    }
    
    func textFieldShouldReturn(_ textField: UITextField) -> Bool {
        nameLabel.text = textField.text
        return true
    }
}
```
UITextFieldDelegate 에 있는 대로 구현해줍니다. Xcode에서 자동으로 완성해주니 엄청 편리하네요. `code` 부분에 위에서 버튼을 클릭 했을 때 했던 행동을 넣어줍니다. 
그렇다면, textField에 포커스가 가 있는 상태에서 엔터키가 눌리면, textField에 있는 문자열이 nameLabel에 출력될 것 입니다. 해봅시다.
완성!

### 정리 
Delegate 패턴은 처음엔 어려웠지만, 오히려 내가 어떤 이벤트들을 탐지하고 위임받아 컨트롤 할 수 있는지 쉽게 알 수 있어서 좋았습니다. 앞으로는 Delegate를 잘 읽어서 구현하고자 하는 용도에 맞게 구현해여겠습니다.


