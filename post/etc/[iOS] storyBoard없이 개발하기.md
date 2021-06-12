UIViewController를 storyBoard없이 만들기

IOS를 개발하다보니, storyBoard를 가지고 개발하는 경우와 없이 개발하는 방법 두 가지가 혼재되어있어, 이번 기회에 storyBoard없이 개발하는 방법에 대해 공부해보려 한다.

스토리 보드 없이 코드만으로 화면을 구성하려면, 스토리 보드만 지우면 되는게 아닌 메인 인터페이스 설정 및 코드상에서 window에 대한 설정을, 진입점을 잡아줘야 한다.

### 환경 설정
1. 스토리 보드 지우기 -> 실행 (Thread 1: Exception: "Could not find a storyboard named 'Main' in bundle NSBundle)
2. Project -> General -> Main interface -> 공백입력
3. info.plist -> Application Scene Manifest -> Storyboard Name -> 줄 전체 삭제 -> 실행 (Thread 1: Exception: "Invalid parameter not satisfying:)
4. iOS 13 이상만) iOS 13 이전에는 모든 장면 기능이 AppDelegate.swift 클래스에 있었으며 이제는 SceneDelegate.swift라는 별도의 클래스에 있습니다. `SceneDelegate.swift`에서 프로젝트를 설정하겠습니다.
5. 아래와 같은 코드를 `SceneDelegate.swift`에서 작성(수정)합니다.
```
func scene(_ scene: UIScene, willConnectTo session: UISceneSession, options connectionOptions: UIScene.ConnectionOptions) {
        guard let windowScene = (scene as? UIWindowScene) else { return }

        window = UIWindow(frame: windowScene.coordinateSpace.bounds)
        window?.windowScene = windowScene
        window?.rootViewController = ViewController()
        window?.makeKeyAndVisible()
    }
```

### UIViewController 띄우기
```
class ViewController: UIViewController {

    override func viewDidLoad() {
        super.viewDidLoad()
        view.backgroundColor = .blue
    }
}
```


