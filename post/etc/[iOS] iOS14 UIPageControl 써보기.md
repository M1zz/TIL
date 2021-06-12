기존의 UIPageControl과 다른 부분이 추가되어서 대응을 해야 할 부분이 있는지 살펴보았습니다.  
UIPageControl 소스코드에 들어가서 살펴보도록 하겠습니다. 
여러분의 페이지 컨트롤이 클릭을 통해 이동이 가능하다면, 아마 iOS14 업데이트에서 영향을 받았을 확률이 큽니다.
주석의 내용들은 삭제 해 두었으니 궁금하신 분들은 한번 찾아가서 읽어봐도 좋을 듯 합니다.

### 새로운 상태, 스타일
UIPageControl의 새로운 상태와, 백그라운드 스타일이 추가되었습니다.
0은 아무런 인터랙션이 없는 상태, 1은 1칸을 움직이는 상태, 2는 꾹 누른상태에서 주르르륵
이동하는 상태 입니다.

배경같은 경우 automatic은 꾹 누르지 않았을 때는 투명 꾹 누르면 반투명인 상태가 됩니다.
prominent이면 반투명, minimal이면 투명입니다.

```
@available(iOS 14.0, *)
    public enum InteractionState : Int {
        case none = 0
        case discrete = 1
        case continuous = 2
    }

    @available(iOS 14.0, *)
    public enum BackgroundStyle : Int {
        case automatic = 0
        case prominent = 1
        case minimal = 2
    }
}
```
### 프로퍼티들
`backgroundStyle` 은 위의 설명에 따른 페이지 컨트롤의 배경설정을 가능하게 합니다.  
`interactionState` 상태를 읽을 수 있게 해줍니다.  
`allowsContinuousInteraction` 연속적인 인터렉션이 가능하게 할 것인지에 대한 여부입니다.  
`preferredIndicatorImage` 커스텀 페이지 뷰를 만들 수 있습니다.  
`indicatorImage` 인디케이터 이미지도 설정할 수 있습니다.  
```
@available(iOS 14.0, *)
open var backgroundStyle: UIPageControl.BackgroundStyle

@available(iOS 14.0, *)
open var interactionState: UIPageControl.InteractionState { get }

@available(iOS 14.0, *)
open var allowsContinuousInteraction: Bool

@available(iOS 14.0, *)
open var preferredIndicatorImage: UIImage?

@available(iOS 14.0, *)
open func indicatorImage(forPage page: Int) -> UIImage?

@available(iOS 14.0, *)
open func setIndicatorImage(_ image: UIImage?, forPage page: Int)
```

한번 예제 코드를 만들어 보겠습니다. 페이지의 갯수를 100개로 임미로 만든 다음에, 
꾹 눌러서 연속적으로 페이지 이동 가능하게 하려면 다음과 같이 값을 대입해줍니다. pageControl.allowsContinuousInteraction = true 

배경은 언제나 투명하도록 설정하겠습니다. pageControl.backgroundStyle = .minimal 를 이용해서 할 수 있습니다.
또한 pageControl.preferredIndicatorImage 를 이용해 커스텀 페이지 컨트롤을 만들어 보겠습니다.
예제 코드는 아래와 같습니다.
```
import UIKit

class ViewController: UIViewController {
    @IBOutlet weak var pageControl: UIPageControl!
    
    override func viewDidLoad() {
        super.viewDidLoad()
        // Do any additional setup after loading the view.
        pageControl.numberOfPages = 100
        if #available(iOS 14.0, *) {
            pageControl.allowsContinuousInteraction = true
            pageControl.backgroundStyle = .minimal
            pageControl.preferredIndicatorImage = UIImage.init(systemName: "heart.fill")
        }
    }
    @IBAction func click(_ sender: Any) {
        if #available(iOS 14.0, *) {
            print(pageControl.interactionState.rawValue)
        }
    }
}
```
