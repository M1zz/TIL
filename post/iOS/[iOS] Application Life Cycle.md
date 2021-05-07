앱의 생명주기는 앱의 사용과정 중 가지는 상태값 입니다.  
foreground, background을 오가며 앱의 상태값이 바뀝니다.  
크게 5개의 상태를 가지고 개발자가 상태에 맞게 개발할 수 있도록 인터페이스를 제공합니다.  

### 상태
- Not Running(Terminated) : 앱이 시스템에 의해 완전히 종료된 상태입니다. 혹은 아직 실행되지 않았을 때도 Not Running 상태입니다.
- Inactive(Foreground) : 앱이 실행 중인 상태입니다. 하지만 이벤트를 받지는 않습니다. Active 상태로 넘어가기 전에 앱은 반드시 이 상태를 거칩니다. 알림 같은 특정 알림창이 화면을 덮어서 앱이 event를 받지 못하는 상태가 여기에 해당됩니다.  
- Active(Foreground) : 앱이 실행 중이고 이벤트를 받을 수 있는 상태입니다. Foreground 상태에 있는 앱들은 보통 이 상태입니다.  
- Background : 앱 사용중에 다른 앱을 실행하거나 홈 화면으로 나갔을 때 상태입니다. 백그라운드에서 동작하는 코드를 추가하면 suspended 상태로 넘어가지 않고 백그라운드 상태를 유지하게 됩니다. 처음부터 background 상태로 실행되는 앱은 inactive 대신 background 상태로 진입합니다. 음악을 실행하고 홈 화면으로 나가도 음악이 나오는 상태가 이 경우에 해당됩니다.  
- Suspended : 앱이 background 상태에서 추가적인 작업을 하지 않으면 곧바로 suspended 상태로 진입합니다. 앱을 다시 실행할 경우 빠른 실행을 위해 메모리에만 올라가 있습니다. 메모리가 부족한 상황이 되면 iOS는 suspended 상태에 있는 앱들을 메모리에서 해제시켜서 메모리를 확보합니다.


### OverView
전체적인 그림을 놓고 봤을 때 Not Running 상태에서 Inactive 전환되기까지 거치는 단계가 있다. 그리고 Inactive에서 Active로 전환되고, 앱이 동작합니다.  
앱이 동작하는 Active에서 Background로 전환된기 전에 Inactive로 전환됩니다. 그리고 추가적인 작업을 하지 않으면 곧바로 suspended 상태로 진입합니다.  
백그라운드에서 동작하는 코드를 추가하면 suspended 상태로 넘어가지 않고 백그라운드 상태를 유지하게 됩니다. 그리고 다시 Active로 가기 전에는 Inactive를 거쳐 갑니다.

### Not Running(Launch)
앱의 아이콘이 클릭되면, main()함수가 실행되고, UIApplicationMain()이 호출됩니다. 그리고 Main UI file을 불러오고 초기화가 시작됩니다.  

##### application(_:willFinishLaunchingWithOptions:)  
- 앱에 필요한 주요 객체들을 생성하고 run loop를 생성하는 등 앱의 실행 준비가 끝나기 직전에 호출됩니다.
- App State : Launch Time(Not Running)

##### application(_:didFinishLaunchingWithOptions:)  
- 앱 실행을 위한 모든 준비가 끝난 후 화면이 사용자에게 보여지기 직전에 호출됩니다. 주로 최종 초기화 코드를 작성합니다.
- App State : Launch Time(Not Running)

### Inactive(Foreground)
앱이 백그라운드로 가기 전 또는 액티브 상태로 가기 전의 상태입니다.
##### applicationWillEnterForeground(_:)
- 앱이 Background 또는 Not-Running 상태에서 Foreground로 들어가기 직전에 호출됩니다.
- App State : Background or Not Running -> In-Active -> Active

##### applicationDidEnterBackground(_:)
- 앱이 Foreground 상태에서 Background로 들어가기 직전에 호출됩니다.
- App State : Active -> In-Active -> Background

### Active(Foreground)
앱이 In-Active 상태에서 Active로 전환되고 실행됩니다.  
Active 상태에서 View의 동작 순서를 보려면 [뷰 컨트롤러의 생명주기를](https://dev200ok.blogspot.com/2020/06/ios.html) 확인해주세요.
##### applicationDidBecomeActive(_:)
- 앱이 active 상태로 진입하고 난 직후 호출됩니다. 앱이 실제로 사용되기 전에 마지막으로 준비할 수 있는 코드를 작성할 수 있습니다.
- App State : Active

##### applicationWillResignActive(_:)
- 앱이 Active에서 In-Active 상태로 진입하기 직전에 호출됩니다. 주로 앱이 quiescent(조용한) 상태로 변환될 때의 작업을 진행합니다.
- App State : Active -> In-Active -> Background

### Background
##### applicationDidEnterBackground(_:)
- 앱이 background 상태에 진입 직후 호출됩니다. 앱이 Suspended 상태로 진입하기 전에 중요한 데이터를 저장하거나 점유하고 있는 공유 자원을 해제하는 등 종료되기 전에 필요한 준비 작업을 진행합니다.
- 특별한 처리가 없으면 background 상태에서 곧바로 suspended 상태로 전환됩니다.
- 앱이 종료된 이후 다시 실행될 때 직전 상태를 복구할 수 있는 정보를 저장할 수 있습니다.
- App State : Background

### Suspended
##### applicationWillTerminate(_:)
앱이 종료되기 직전에 호출출되며 앱이 곧 종료될 것임을 알려줍니다.
- 사용자가 직접 앱을 종료할 때만 호출됩니다. 다음 경우에는 호출되지 않습니다.
  1. 메모리 확보를 위해 Suspended 상태에 있는 app이 종료될 때
  2. Background 상태에서 사용자에 의해 종료될 때
  3. 오류로 인해 app이 종료될 때
  4. Deivce를 재부팅할 때
- App State : Active -> Terminate

참고 : https://velog.io/@cskim/iOS-App-%EC%83%9D%EB%AA%85%EC%A3%BC%EA%B8%B0Life-Cycle
