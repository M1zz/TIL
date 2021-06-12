스토리 보드 없이 뷰 컨트롤러를 호출하면 viewDidload()에서 이런 저런 설정을 해준다.
`viewDidload`는 언제 호출되고, 이 다음과 이전에 호출되는 것이 또 있을까? 라는 궁금증을 가지고 검색을 했더니 viewController life-cycle이라는 키워드를 찾았다.

### Life Cycle
뷰 컨트롤러는 어떤 생명주기를 가지고 있을까?
1. init
2. 2 loadView
3. viewDidLoad
4. viewWillAppear
5. viewDidAppear
6. viewWillDisappear
7. viewDidDisappear -> 4 viewWillAppear
8. viewDidUnload

이런 생명주기를 가진다.

가장 먼저 뷰 컨트롤러에 구현되어있던 viewDidLoad를 시작으로 살펴보면, 로드 -> 나타남 -> 사라짐 -> 언로드의 순서를 가진다.
Did와 Will로 전후에 호출되는 함수를 구별한다. 예를 들면 viewDidLoad는 뷰가 로드 되었다. 그 다음에 viewWillAppear는 뷰가 나타날 것이다. 와 같이 이름만 보아도 언제 호출되는지 알 수 있도록 만들어놓았다.

### viewDidload
뷰 로드 완료 후 자동을 호출된다. 리소스의 초기화에 많이 사용된다. 뷰가 처음 만들어질 때 한 번만 실행된다.

### viewWillAppear
얼핏 보면 viewDidload와 같은 기능을 하는 것 처럼 보인다. 실제도 viewDidload 바로 다음에 호출된다. viewDidload 와의 차이점은 위의 생명주기의 `viewDidDisappear -> viewWillAppear`에서 나타난다. 뷰1:메인페이지, 뷰2:상세페이지가 있다고 가정하자. 화면의 이동이 있을 때, 메인페이지에서 상세페이지로 갔다가 다시 메인으로 돌아오면 이미 그려진(viewDidload가 한번 호출 된) 메인페이지는 다시 viewDidload가 호출되지 않는다. viewWillAppear가 호출된다. 그러므로 페이지가 나타날 때마다 해야할 행동과 뷰가 로드될 때 해야할 행동으로 나누어서 코드를 작성해야한다.

### viewDidAppear
뷰가 화면에 나타난 직후에 실행된다. 그러므로 화면이 그려진다음 적용 될 애니메이션을 그리는데 사용된다.

### viewWillDisappear
뷰가 사라지려고 할 때 뷰 컨트롤러에게 알려준다. 사용자가 뷰에서 떠나려는 행동을 하는 것을 알고 싶을 때 사용하면 된다.

### viewDidDisappear
뷰가 사라졌음을 알려준다. 실제 화면 전환에서 살펴보자.
뷰1:viewWillDisappear -> 뷰1:viewDidDisappear

이렇게 되는 순서는 맞지만 화면 전환은 다른 화면도 있다는 뜻이다. 다른 화면의 로딩까지 순서대로 맞춰 보자.
뷰1:viewWillDisappear -> 뷰2:viewDidload -> 뷰2:viewWillAppear -> 뷰1:viewDidDisappear

해석 해보면, 뷰1이 사라 질 것이다라고 알림을 준다. 그 다음 뷰2가 로딩이 되고 이제 나타날 것이다라는 알림이 간 후 뷰1이 완전히 사라진다. 그리고 그 다음 순서는 뷰2:viewDidAppear 이다.

### viewLoad
애플에서 해당 메서드는 직접 호출하면 안된다. 라고 규정짓고 있습니다. 뷰의 초기화 작업은 viewDidLoad에서 하십시오. 라고 제안하고 있다. loadView가 뷰를 만든 다음에 메모리에 올라가면 그 때 viewDidLoad가 호출된다.

