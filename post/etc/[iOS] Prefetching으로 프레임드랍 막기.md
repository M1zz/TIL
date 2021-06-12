### 필요한 이유  
컬렉션 뷰에서 cellForRowAt 메소드로 데이터를 가져와서 컬렉션 뷰를 그릴 경우, 
셀을 구성하는 작업은 메인 쓰레드에서 진행 될 것입니다.   
많은 셀을 메인 쓰레드에서 진행한다면, 프레임 드랍이 일어날 수 있고 
결과적으로 매끄러운 스크롤링이 불가능합니다.  

이런 작업을 돕기 위해 Prefetching이 추가되었습니다. 
사용 필요 요건은 다음과 같습니다.  
iOS 10.0+  
Xcode 11.3+  

### 비교
그럼 그냥 컬렉션 뷰만 쓸 때와는 어떤점이 다를까요? 사실 이미지나, 
서버에 요청해서 데이터를 받아오지 않아서인지 큰 차이를 느끼지 못했습니다.  
하지만 구현 해놓고 브레이킹 포인트를 걸고, 출력을 해 보면서 차이를 느낄 수 있었습니다.

가장 먼저 제가 다름을 느낀 곳은 같은 화면을 구성하는데 프리패칭 프로토콜을 채택해서 구현 했느냐, 
아니냐에 따라 데이터의 fetching 갯수가 다른 것 이었습니다.  
왜냐하면 cellForRowAt에서는 셀을 그리기 위한 작업만을 해서 그런지 그리기 위한 데이터만 가져오는 것 처럼 보였습니다.  
하지만 프리패칭 프로토콜을 채택하면, 디스플레이 영역이 아닌, 프리패칭의 영역의 데이터를 미리 가져옵니다.
그렇기 때문에 가져오는 데이터의 갯수가 다릅니다.  

화면에 6개가 그려져야 할 때, Prefetching 없이 컬렉션 뷰로 그리게 되면 일단 화면에 그려지는 6개의 셀의 데이터만
가져오고 아래로 스크롤 할 때 새로운 데이터를 로드 합니다.

하지만 이 갯수가 중요한게 아닙니다. 이 그림을 그리는 작업이 어디서 이루어지고 있는지가 핵심입니다.  
셀을 그리고 있는 cellForRowAt작업은 메인 스레드에서 이루어지는데 해야 할 일들이 많아지면 다 처리하지 못하고 
프레임 드랍이 일어날 수 있다. 이 작업 중 일부가 백그라운드에서 일어나게 되면 메인 쓰레드에 걸리는 부하가 줄어들고 
프레임 드랍 문제를 해결 할 수 있습니다.

### 구현
prefetchRowsAt는 눈에 보이는 셀의 데이터를 가져오지는 않습니다. 초기화 된 셀이 거의 그려질 무렵, prefetchRowsAt가 호출되고 보이지 않는 셀들의 데이터를 불러오기 시작합니다.  
그리고 스크롤링이 될 때 보이지 않는 셀은 보이게 되며, 그 뒤에 있는 셀의 데이터가 필요해집니다. 이 때 prefetchRowsAt 메소드가 호출되고, 데이터를 불러오게 됩니다.
그리고 셀이 그려지면서 cellForRowAt이 호출됩니다.
prefetchRowsAt는 cellForRowAt보다 먼저 불리며, cellForRowAt에서 셀을 그릴 때 필요한 데이터를 프리 패칭 하도록 돕는 메소드입니다.  
그러니 비동기로 구현해 주어야겠죠? 

비동기로 구현한 코드는 애플에서 잘 정리해서 제공해주니 참고하면 많은 도움이 될 것 같습니다.


참고 자료  
[What's New in Cocoa Touch ](https://developer.apple.com/videos/play/wwdc2018/202/)  
[Prefetching for UITableView](https://andreygordeev.com/2017/02/20/uitableview-prefetching/)
[Prefetching Collection View Data](https://developer.apple.com/documentation/uikit/uicollectionviewdatasourceprefetching/prefetching_collection_view_data)
