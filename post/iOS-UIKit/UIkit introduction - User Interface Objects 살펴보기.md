# UIkit introduction - User Interface Objects 살펴보기
![uikit](https://devimages-cdn.apple.com/wwdc-services/articles/images/7543212D-6CBF-496C-A20E-D04E99C3A1DB/2048.jpeg)
## OverView
iOS 앱을 처음 개발하면, 무엇인지도 모르고 공부하기전에 먼저 써보게 됩니다. 알고 쓰는 것보다, 쓰고나서 알아가는게 더 좋다고도 생각합니다. 그래도 어떤 것들이 있고 그래서 무엇을 해주는 것인지 정리해보려 합니다.

UIKit에 대한 사용경험이 있다고 전제하여, 작성되었습니다. 없으시다면 TableView를 만들어 보거나, NavigationController를 사용해 보는 것을 추천드립니다.
제가 공부하는 방식은 일단 많이 만들어보고, 좀 더 자세하게 공식문서를 읽으면서 정리하고, 다시 사용하면서 내것으로 만드는 것 입니다.

UIkit은 iOS개발에 필요한 기본적인 것들을 제공 해 줍니다. 화면을 그리는데 필요한 것들과 사용자의 이벤트를 처리하는데 필요한 것들이죠. 예를들면 터치 이벤트와 같은 것들입니다.

다음은 UIKit을 사용하는데 종요한 내용입니다. 지금당장은 이해가 되지 않아도 알고 있으면 언젠가 깨달음을 얻을 수 있습니다.
```
특별한 경우를 제외하고는 앱의 main thread 혹은 main dispatch queue에서만 UIKit을 사용하세요. 이러한 제한사항은 특별히 UIResponder에서 만들어지거나 어떤식으로든 앱의 사용자 인터페이스를 조작하는 것과 관련된 클래스에 적용됩니다.
```

기본적으로 AppIcon, Launch screen storyboard가 제공됩니다. 이 두가지는 앱을 구성하는 필수 요소이기 때문에 빈화면으로 제공되지만 다른 앱들과 구분할 수 있도록 제공해야 합니다.

Info.plist에는 앱의 Meta data들이 들어있습니다. 여러 설정값들을 이 파일을 수정하여 적용할 수 있습니다.

UIKit의 코드는 MVC 패턴으로 되어있습니다. 간단하게 설명드리자면, 아래의 구조와 같습니다.

![MVC](https://docs-assets.developer.apple.com/published/4e7c26b6ad/ff7aa08f-4857-44ce-88d5-7dacbef84509.png)

데이터들은 모델링을 통해 어플리케이션에서 사용될 수 있습니다. 모델링이 왜 필요한지는 없이 앱을 만들어보시면, 보다 명확함을 위해 필요함을 느낄 수 있습니다. 그리고 이 모델링을 가지고 컨트롤러는 뷰를 컨트롤 합니다. 가공을 하던지, 이벤트를 처리해서 화면에 뿌려 줍니다. 간단한 예를 들면, 사용자의 정보 데이터는 이름, 나이, 전화번호와 같은 모델을 통해 모델링 되고, 컨트롤러는 사용자 정보의 이름, 나이, 전화번호를 view에 사용자의 터치 이벤트를 처리해서 전달해줍니다. 그러면 view에서는 데이터를 뿌려주는 구조입니다. 글로 이해하는 것보다는 좋은 예시 코드를 구해서 코딩을 해보시는 것을 추천 드립니다.

그러면 UIKit을 사용해 어떤 화면을 그릴 수 있는지, 어떤 이벤트를 처리할 수 있는지 한 번 알아봅시다.

개발자 문서는 [이곳](https://developer.apple.com/documentation/uikit)에 있습니다. 직접 꼼꼼하게 읽어보는 것을 추천드려요!

## User Interface - Controllers

### Container view controller
하나의 뷰만 보여줄 때는 하나의 컨트롤러만 있으면 가능하지만, 여러 화면을 보여주어야 한다면 이를 도와주는 도구가 필요합니다. 뷰를 포함하는 컨테이너 뷰 컨트롤러 입니다. 사용자가 앱을 사용하는 인터페이스인 컨테이너뷰들을 나열해보겠습니다. 더 다양하고 많지만 처음에 필요한 녀석들만 나열하겠습니다.

- [UINavigationController](https://developer.apple.com/documentation/uikit/uinavigationcontroller)
- [UITabBarController](https://developer.apple.com/documentation/uikit/uitabbarcontroller)

다른 것도 있지만 처음에는 이 두개의 컨네이터를 공부하면 좋을 것 같습니다. 다른 컨텐츠 뷰 컨트롤러를 포함해 앱의 페이지 이동을 용이하게 해줍니다.


### Content view controller
컨텐츠 뷰 컨트롤러는 컨테이너에 들어가서 다른 컨텐츠를 보여주는 뷰 입니다.
- [UIViewController](https://developer.apple.com/documentation/uikit/uiviewcontroller)
- [UITableViewController](https://developer.apple.com/documentation/uikit/uitableviewcontroller)
- [UICollectionViewController](https://developer.apple.com/documentation/uikit/uicollectionviewcontroller)

테이블 뷰 컨트롤러와 컬렉션 뷰 컨트롤러만 잘 해도 앱의 80%는 만들 수 있다는 이야기가 있을만큼, 기초적이고 많은 내용과 기능이 있으니 따로 정리하겠습니다. 전반적인 큰 그림을 그릴 수 있도록 아웃라인만 소개 해 드릴게요


### Search Interface
테이블 뷰와 컬렉션 뷰를 사용하면, 여러 컨텐츠 들이 나열됩니다. 이를 보다 편하게 사용하기 위해서는 항상 검색의 기능이 요구됩니다.
`UISearchController`는 검색의 기능을 사용하기 위해 제공됩니다. 이 또한 내용이 많으니 여기서 다루지는 않겠습니다.
- [UISearchContainerViewController](https://developer.apple.com/documentation/uikit/uisearchcontainerviewcontroller)
- [UISearchController](https://developer.apple.com/documentation/uikit/uisearchcontroller)

더 많은 컨트롤러들은 공식 문서에서 확인하고 공부하시면 좋을 것 같습니다.

## User Interface - Views

### Container Views
뷰들을 포함할 수 있는 뷰 입니다. 주로 레이아웃을 만들 때 사용됩니다. 아래로, 혹은 아이템을 나열할 때 사용되니 큰 그림을 그릴 때 어떻게 쓰면 좋겠다라는 생각으로 살펴보았습니다.

- [Collection Views](https://developer.apple.com/documentation/uikit/views_and_controls/collection_views)
- [Table Views](https://developer.apple.com/documentation/uikit/views_and_controls/table_views)
- [UIStackView](https://developer.apple.com/documentation/uikit/uistackview)
- [UIScrollView](https://developer.apple.com/documentation/uikit/uiscrollview)

### Content Views
실제 컨텐츠들이 들어가는 뷰들 입니다. 이미지나, 피커들이 있습니다.
- [UIActivityIndicatorView](https://developer.apple.com/documentation/uikit/uiactivityindicatorview)
- [UIImageView](https://developer.apple.com/documentation/uikit/uiimageview)
- [UIPickerView](https://developer.apple.com/documentation/uikit/uipickerview)
- [UIProgressView](https://developer.apple.com/documentation/uikit/uiprogressview)

### Controls
UIView를 포함한, 컨트롤가능한 뷰들이 있습니다.
- [UIControl](https://developer.apple.com/documentation/uikit/uicontrol) - [UIView](https://developer.apple.com/documentation/uikit/uiview)는 모든 Controls의 최상위 부모 클래스 입니다.
- [UIButton](https://developer.apple.com/documentation/uikit/uibutton)
- [UIColorWell](https://developer.apple.com/documentation/uikit/uicolorwell)
- [UIDatePicker](https://developer.apple.com/documentation/uikit/uidatepicker)
- [UIPageControl](https://developer.apple.com/documentation/uikit/uipagecontrol)
- [UISegmentedControl](https://developer.apple.com/documentation/uikit/uisegmentedcontrol)
- [UISlider](https://developer.apple.com/documentation/uikit/uislider)
- [UIStepper](https://developer.apple.com/documentation/uikit/uistepper)
- [UISwitch](https://developer.apple.com/documentation/uikit/uiswitch)

### Text Views
텍스트를 입력 받거나 표현할 때 사용되는 뷰들입니다.
- [UILabel](https://developer.apple.com/documentation/uikit/uilabel)
- [UITextField](https://developer.apple.com/documentation/uikit/uitextfield)
- [UITextView](https://developer.apple.com/documentation/uikit/uitextview)

## User Interactions
사용자의 이벤트를 처리하는 방법에 대해서는 초반부터 깊이 공부할 필요가 없다고 생각되어 리스트만 나열해 두겠습니다. 더 많은 사용자 인터랙션이 있지만, 초반에는 이정도만 살펴보도록 합시다.

- [Touches, Presses, and Gestures](https://developer.apple.com/documentation/uikit/touches_presses_and_gestures)
- [Drag and Drop](https://developer.apple.com/documentation/uikit/drag_and_drop)
- [Pointer Interactions](https://developer.apple.com/documentation/uikit/pointer_interactions)


## 정리

간단하게 리스트만 정리해서 overView만 소개하려 했는데, 생각보다 세세하게 나열하게 되었습니다. 그만큼 내용이 많고 직접 해 봐야 하는 내용이 많다는 뜻 이겠죠?

저 같은 경우에는 searchBar가 있는지도 모르고, UITextField에 이미지를 삽입해서 구현을 했던 경험이 있는데요, 먼저 어떤 것들이 있고, 화면을 그릴 때 떠올릴 수 있다면 성공입니다.

아래의 그림은 인터넷에 떠도는 상속관계를 정리 해둔 그림입니다. 컴포넌트들이 어떻게 되어있는지 안다음에는, 커스텀하게 만들 때 무엇을 가져다써야하는지 살펴보면 좋을 것 같습니다.
모든 것을 꼼꼼하게 본다면, 공부해야한다는 생각만으로 나가 떨어지게됩니다.

전체적인 큰 그림만 보고 계속해서 채워나간다는 생각으로 경험을 쌓아야 할 필요성을 느꼈습니다. 이 글을 쓰면서도 처음 본 object들이 있네요.

혹시 내용이 잘 못 되었거나 수정이 필요한 부분은 코멘트로 남겨주세요.

![structure](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2F4aEas%2Fbtqz7o0Dt9c%2FyZUlQFSpNeiVWmx6RMyoq1%2Fimg.jpg)
