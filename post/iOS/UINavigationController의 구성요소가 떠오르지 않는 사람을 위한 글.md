# UINavigationController의 구성요소가 떠오르지 않는 사람을 위한 글

한 화면으로 이루어져 있는 앱을 만들다 보면, 다른 기능을 새로운 화면에 구성하고 싶은 생각이 듭니다. 이럴 때 필요한것이 화면 전화이죠.
화면 전환에는 여러가지 방법이 있으나, 그 중에 하나인, 가장 기본적이고 많은 기능을 제공하는 NavigationBar의 구성요소와 기능들에 대해 알아보도록 하겠습니다.

## 정의 및 구성요소
`UINavigationBar`는 `UINavigationController`라는 컨트롤러에 의해 제어됩니다. `UIViewController`를 상속받아 구현되어 있기 때문에 `UIViewController`가 가진 요소들을 가지고 있습니다. 가장먼저 `UINavigationController`에 대해 먼저 알아보겠습니다.
```
class UINavigationController : UIViewController
```
`UINavigationController`는 하나의 뷰 컨트롤러가 아닌 여러개의 뷰 컨트롤러의 계층을 만들고 관리 해주는 컨트롤러 입니다. 사용자가 상세화면이나 상위에 있는 화면이라고 생각하게 만들어주죠. 아래의 이미지를 보면서 생각해봅시다.

![UINavigationController](https://docs-assets.developer.apple.com/published/83ef757907/navigation_interface_2x_8f059f7f-2e2f-4c86-8468-7402b7b3cfe0.png)

첫 화면에서 `General`버튼을 누르면, `General` 페이지로 이동됩니다. 그려먼 화면의 타이틀이 바뀌고, 이전 페이지로 돌아갈 수 있는 버튼이 생기죠. 실제 화면이 쌓이고, 사라지는 순서를 관리하는 `navigation stack`이 있습니다. 네비게이션의 첫 뷰 컨트롤러는 스택의 root인 `root view controller`가 됩니다. 즉 네비게이션 컨트롤러는 `root view controller`인 뷰컨트롤러가 필요하죠.


![NavigationBar](https://docs-assets.developer.apple.com/published/dde7452123/3abba22e-4aef-47dd-b4e2-a9965c424338.png)

화면의 순서는 stack이 관리하지만, 사용자가 이 스택을 조종하기 위한 인터페이스로 `UINavigationBar`가 있습니다.
`UINavigationBar`의 구성 요소는 위의 그림과 같습니다. 일반적으로 화면의 가장 상단에서 화면의 계층을 나타내 줍니다. 이전화면은 어디서 왔는지, 내가 할 수 있는 행동은 무엇인지, 이 페이지가 어디인지를 타이틀 등을 활용해서 나타내줍니다.
이미 사용경험이 많은 UI라 익숙하죠?

![NavigationOverView](https://docs-assets.developer.apple.com/published/83ef757907/nav_controllers_objects_a8447aef-d652-4ab9-85d1-1eb8e4876e12.jpg)

위 그림은 `UINavigationController`를 머리에 담을 때 가지고 있으면 좋은 전체의 큰 그림입니다. `UINavigationController`는 여러개의 뷰 컨트롤러들을 `navigation stack`을 통해관리하죠!
그리고 각 뷰컨트롤러들의 전환을 `UINavigationBar`를 이용해서 하게 됩니다.

필요하다면 toolbar를 `UIToolbar`를 이용해 구현할 수 있습니다. 그 이외에 델리게이트로 이벤트 들을 핸들링 할 수 있습니다.

다음으로는 각각의 요소들이 무슨기능을 할 수 있는지 알아보도록 하겠습니다.

## NavigationController
`NavigationController`은 뷰 컨트롤러의 컨테이너 뷰 컨트롤러 입니다. 즉 화면에 보이는 뷰 컨트롤러를 네비게이션 컨트롤러가 감싸고 있습니다. 이게 전부가 아닙니다. 네비게이션바, 그리고 선택적으로 툴바도 있습니다. 이 모든 것들을 조립해서 관리합니다.

![NavigationOverView](https://docs-assets.developer.apple.com/published/83ef757907/NavigationViews_2x_e69e98a2-aaac-477e-9e33-92e633e29cc7.png)

화면이 바뀌면 덩달아, 네비게이션 바도 업데이트가 되어야 하고, 툴바 또한 업데이트가 되어야 합니다. 사용 했던 경험을 생각해보면, 상세페이지로 넘어갔는데 title이 바뀌지 않는다면 이상함을 느낄테니까요.

UINavigationController의 구현체를 조금 살펴보겠습니다.

- pushViewController
  - navigationstar에 뷰컨트롤러를 push 합니다.
- popViewController
  - navigationstar에 뷰컨트롤러를 pop 합니다.
- viewControllers
  - 현재 viewControllers를 가리킵니다.
- navigationBar
  - 네비게이션바를 지칭합니다.
- isNavigationBarHidden
  - 네비게이션바의 보임 여부를 나타냅니다.
- toolbar
  - 툴바를 지칭합니다.
- isToolbarHidden
  - 툴바의 보임 여부를 나타냅니다. 기본은 숨김입니다.

이 외에 무엇이 있고, 어떤 기능을 하는지 한 번쯤은 살펴보는 것이 좋다고 느끼고 있습니다. 실제 코딩을 할 때, 저런 프로퍼티와 메소드가 있는지도 모르고 구현했던 경험이 많습니다. 나중에는 내가 필요한 건 이미 있지 않을까 하는 생각이 먼저 들기 시작했고, 그래서 이런 글을 써서 정리하고 있습빈다.

## NavigationBar, NavigationItem
`NavigationController`안에 보면, `NavigationBar`과 `NavigationItem` 둘 다 있습니다. 이 부분이 계속 헷갈리고 네비게이션 바를 제대로 사용하지 못하게 되는 원인이라 확실하게 스스로 가지고 있는 궁금증을 정리하려고 합니다.

### 1. 언제 NavigationBar를 써야하고 언제 NavigationItem를 써야하나요?
```
navigationController?.navigationBar.backgroundColor = UIColor.white
navigationController?.navigationBar.isTranslucent = false
```
```
navigationItem.hidesBackButton = true
navigationItem.leftItemsSupplementBackButton = true
navigationItem.leftBarButtonItem = backButtonItem
```
보통 위와 같은 코드를 많이 보아서, 언제 무엇을 써야하는지 헷갈립니다.
왜 이런 코드들이 나오는지를 알기 위해서 `navigationController` 구현체를 뜯어보면, 아래쪽에 `UIViewController`의 `extension`이 있습니다.

```
extension UIViewController {
    open var navigationItem: UINavigationItem { get } // Created on-demand so that a view controller may customize its navigation appearance.

    open var hidesBottomBarWhenPushed: Bool // If YES, then when this view controller is pushed into a controller hierarchy with a bottom bar (like a tab bar), the bottom bar will slide out. Default is NO.

    open var navigationController: UINavigationController? { get } // If this view controller has been pushed onto a navigation controller, return it.
}
```
네비게이션 바를 사용할 때, 다른 뷰와는 달리 subview를 추가하는 방법이 아닌, navigationItem을 사용해야 합니다. 이게 무슨말이냐면, 자유롭게 원하는 걸 추가하는것이 아닌, 이미 정해진 곳에만 네비게이션 바를 커스터마이징 할 수 있다는 뜻 입니다. 그래서 `navigationItem.leftBarButtonItem` 이라는 정해진 위치에 넣게 되는거죠. 참고로 `searchController`도 `navigationItem`에 있습니다. 그러므로 네비게이션에 서치바를 넣으려면 아래와 같이 해주면 되겠죠?
```
let searchController = UISearchController(searchResultsController: nil)
self.navigationItem.searchController = searchController
```

정리해보면, 네비게이션 바의 틴트 색상이나, 배경색과 같이 바 자체의 모양이과 외형에 대한 값을 설정할 때는, `navigationBar`를 사용하고, 네비게이션 바의 기능 동작을 위한 새로운 아이템을 추가하고 기능을 구현할 때는 `NavigationItem`에 `rightBarButtonItems`나 `leftBarButtonItems`에 커스텀한 버튼들을 넣어우어 해결할 수 있습니다.


### 2. UINavigationBar에는 어떤 것들을 설정할 수 있나요?
`UINavigationBar`의 인스턴스입니다. `UIView`를 상속했습니다. 블로그를 읽을 때 왜 무엇을 상속했는지 써둔 내용을 별 의미없이 넘겼었는데, 나중에 돌아보니 어떤 프로퍼티가 있고 메소드가 있습니다를 다 정리하면 한 문장으로 OO를 상속했습니다가 되는구요. 그 후로는 어떤 클래스를 상속했는지 유심히 보고 있습니다.

![NavigationBar](https://docs-assets.developer.apple.com/published/dde7452123/3abba22e-4aef-47dd-b4e2-a9965c424338.png)

- barStyle
- isTranslucent
- topItem
- backItem
- prefersLargeTitles
- tintColor
- barTintColor
- setBackgroundImage()
- titleTextAttributes
- largeTitleTextAttributes

등이 있습니다. 위의 값을 가져오거나 설정해야 할 일이 있을 때 NavigationBar를 사용합시다.

### 3. NavigationItem에는 어떤 기능이 있나요?

- title
- titleView
- backBarButtonItem
- backButtonTitle
- hidesBackButton
- leftBarButtonItem
- rightBarButtonItem
- largeTitleDisplayMode
- searchController
- hidesSearchBarWhenScrolling

와 같은 기능이 필요할 때 사용합니다. 좀 더 자세한 내용은 직접 코드에서 구현체를 살펴보는 좋을 것 같습니다.

## 정리
네비게이션컨트롤러 및 뷰 컨트롤러에서, 네비게이션바를 설정하거나, 네비게이션 아이템을 추가할 때 항상 헷갈렸는데 이번 기회에 코드를 다 한번씩을 살펴보면서 어떤 요소들이 구현되어있는지 살펴보았습니다. 필요한 기능을 찾아서 구현하는 것도 좋지만, 내가쓰고있는 SDK가 어떻게 구현되어 있는지 보는것도 다른 설계를 위해 필요하다고 생각합니다. 잘못된 내용이나, 첨부할 만한 내용은 댓글 혹은 이메일로 보내주시면 감사하겠습니다.
