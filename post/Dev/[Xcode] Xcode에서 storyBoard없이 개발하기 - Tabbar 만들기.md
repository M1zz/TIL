개인적으로는 스토리보드를 가지고 개발하는 것이 좋다고 생각합니다. 하지만 스토리 보드 없이 개발을 하면 완벽하게 어떤 과정이 UI적인 storyBoard를 통해서 동작하는지 알 수 있기 때문에 만들면서 공부하면서 기록을 남겨놓습니다.

[스토리 보드없이 개발](https://dev200ok.blogspot.com/2020/06/storyboard.html) 할 수있는 환경을 구축했다면, Tabbar를 사용해서 2개의 tabbarItem을 가진 앱을 만들어 봅시다.

### 구조
스토리 보드가 없기 때문에 어떤 구조를 가지고 있는지 머리에 명확히 가지고 시작해야 합니다.
우리가 만들 앱의 큰 틀은 하나의 tabbar에 2개의 view가 각각 navigationController로 조작을 하는 구조입니다.

### tabbarController 추가
`window?.rootViewController` 부분에 rootViewController를 설정해주어야합니다. 우리는 tabbar를 컨트롤 할 `tabbarController`를 할당해 주겠습니다.

```
let tabbar = UITabBarController()
UITabBar.appearance().tintColor = .systemGreen
tabbar.viewControllers = [네비게이션컨트롤러1, 네비게이션컨트롤러2]
window?.rootViewController = tabbar
```

이렇게 2개의 네비게이션 컨트롤러를 tabbar에 추가해 줄 수 있습니다.

### 각 view추가
tabbar를 만들었으니 이제 각 탭을 넣었을 때, 호출할 view들을 추가해줍니다.
```
// first tabBarItem
let searchVC = SearchVC()
searchVC.title = "search"
searchVC.tabBarItem = UITabBarItem(tabBarSystemItem: .search, tag: 0)
        
UINavigationController(rootViewController: searchVC)

// second tabBarItem
let favoritesListVC = FavoritesListVC()
favoritesListVC.title = "favorites"
favoritesListVC.tabBarItem = UITabBarItem(tabBarSystemItem: .favorites, tag: 1)
        
UINavigationController(rootViewController: favoritesListVC)

tabbar.viewControllers = [UINavigationController(rootViewController: searchVC), UINavigationController(rootViewController: favoritesListVC)]
```
위 코드와 같이 tabbar의 컨트롤러들을 추가할 수 있습니다.

### 정리
먼제 window위에 올라가는 tabbarController를 만들어주고, 탭바의 아이템을 만들어서 네비게이션컨트롤러를 올려줍니다. 각각의 네비게이션 컨트롤러에는 새로운 뷰 컨트롤러를 올려줍니다.

뷰 컨트롤러들의 stack은 아래의 그림과 같이 됩니다. 




[전체 소스코드 살펴보기](https://github.com/M1zz/githubFollower)
