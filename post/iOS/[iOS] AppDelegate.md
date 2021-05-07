`AppDelegate.swift` 는 swift로 IOS 개발을 시작하면서 부터 보였고, 계속 궁금해서 이 기회에 정리해놓으려한다.  

[공식문서 참고](https://developer.apple.com/library/archive/referencelibrary/GettingStarted/DevelopiOSAppsSwift/BuildABasicUI.html#//apple_ref/doc/uid/TP40015214-CH5-SW1)  
[UIApplicationDelegate](https://developer.apple.com/documentation/uikit/uiapplicationdelegate)

`AppDelegate.swift` 크게 두가지 기능을 한다.
1. AppDelegate 클래스를 정의
어플리케이션이 그려질 window를 생성하고, 상태가 변할 때 반응 할 수 있게 한다. 또한  AppDelegate 클래스는 UIApplicationDelegate 프로토콜을 채택해야한다. 

2. 앱에 대한 진입 점과 입력 이벤트를 앱에 전달하는 실행 루프를 생성
이 작업은 파일 상단에 나타나는 UIApplicationMain 의 특성이다. (@UIApplicationMain)에 의해 수행된다.

하지만 IOS13부터는 `AppDelegate.swift`, `SceneDelegate.swift` 두 파일로 나뉘어 생긴다. 그렇기 때문에 여러 소스를 보았을 때 혼란을 야기했다.

이전에는 (~ IOS12) 아래와 같은 구조였다.
- AppDelegate
  - Process Lifecycle
    - App Launched
    - App Terminated
  - UI Lifecycle
    - Entered Foreground
    - Become active

하지만 지금은 (IOS13) 아래와 같다. Session Lifecycle에 대한 역할이 추가되었다. 
- AppDelegate
  - Process Lifecycle
  - Session Lifecycle
    - Session Created
    - Session Discarded  


- SceneDelegate
  - UI Lifecycle
    - Entered Foreground
    - Become active


AppDelegate 클래스에 있던 프로퍼티인 window가 SceneDelegate로 옮겨졌다. 하지만 하나의 화면에서 여러개의 다중 화면을 지원하면서 `scene`개념이 추가되었다, 

### scene
> UIKit은 UIWindowScene 객체를 사용하는 UI의 인스턴스를 관리한다. Scene에는 UI의 하나의 인스턴스를 나타내는 windows와 view controllers가 들어있다. 각 scene에 해당하는 UIWindowSceneDelegate 객체를 가지고 있고, 이 객체는 UIKit와 앱 사이에 상호 작용에 사용한다. Scene들은 같은 메모리와 앱 프로세스 공간을 공유하면서 서로 동시에 실행된다. 결과적으로 하나의 앱은 여러 scene과 scene delegate 객체를 동시에 활성화할 수 있다.

window를 이용해 어플리케이션의 화면을 그린다. window는 옵셔널이기 때문에 특정 시점에 값이 없을 수도있다.
> var window: UIWindow?

AppDelegate 클래스는 다음과 같은 메소드가 구현되어 있었다. 이 메소드를 구현하면, 어플리케이션에 상태변화에 대응할 수 있다.  
> UIApplication   
- func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplicationLaunchOptionsKey: Any]?) -> Bool  
- ~~func applicationWillResignActive(_ application: UIApplication)  ~~
- ~~func applicationDidEnterBackground(_ application: UIApplication)  ~~
- ~~func applicationWillEnterForeground(_ application: UIApplication)  ~~
- ~~func applicationDidBecomeActive(_ application: UIApplication)  ~~
- func applicationWillTerminate(_ application: UIApplication)  

다음과 같이 바뀌었다.
> UISceneDelegate  
- func sceneWillResignActive(_ application: UIApplication)  
- func sceneDidEnterBackground(_ application: UIApplication)  
- func sceneWillEnterForeground(_ application: UIApplication)  
- func sceneDidBecomeActive(_ application: UIApplication)  

### AppDelegate는 어떤 역할?
IOS13 이전에는 앱이 foreground나 background로 이동할 때 앱의 상태를 업데이트와 같이 앱의 Lifecycle 이벤트를 관리했었지만 더이상 하지 않는다.

1. 앱의 중요 데이터 구조를 초기화
2. 앱의 scene의 환경설정
3. 앱 밖에서 발생한 알림(배터리 부족 경고, 다운로드 완료 알림 등)에 대응
4. 특정한 scenes, views, view controllers에 한정하지 않고 앱 자체를 타겟하는 이벤트에 대응
5. 애플 푸쉬 알림과 같이 실행시 요구되는 모든 서비스를 등록



