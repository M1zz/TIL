UItableView를 처음에 공부할 때는, 어떻게 사용하는지, 화면에 테이블만 잘 그려지면 땡 이라는 생각으로 만들었습니다. 하지만 iOS의 개발의 절반 이상이라고 해도 과언아 아닐 정도로 많이 다루는 주제이기 때문에, 무었이 있는지 어떻게 공부를 해야하는지 정리 해 두고 깊게 공부하면서 남겨볼까 합니다. 내용은 전부 [Apple Developer Documentation](https://developer.apple.com/documentation/technologies)에 있는 내용이기 때문에, 더 정확한 내용을 원하시는 분들에게는 원문을 읽어 보시는 것을 추천 드립니다.


첫 번째 글이지만 문서의 순서와는 상관없습니다. 실제 문서에서는 사용자 인터페이스의 하위 항목 > 뷰와 컨트롤 > 컨테이너뷰 > 컬렉션 뷰와 테이블 뷰 중 테이블 뷰의 순서대로 설명합니다.


### 테이블 뷰 요약
테이블 뷰는 컬럼들을 수직의 방향으로 보여주고, 스크롤 해서 볼 수 있도록 제공해 주며, 셀과 섹션으로 이루어져있습니다. `셀`은 한 줄의 구성요소이고, 이런 셀들을 특정 기준으로 묶은 것이 `섹션`입니다.


테이블 뷰는 셀들을 직접 만들 수 있으며, 조립해서 사용할 수 있습니다.
테이블 뷰를 사용할 때는 데이터가 층계구조로 잘 구분될 때 입니다. 그리고 테이블 뷰 컨트롤러를 이용해서 다룰 수 있습니다. 보통은 UITableViewDelegate와 UITableViewDataSource 이 두 프로토콜을 채택해서 프로그래밍을 하게 되는데요, UITableViewDelegate에는 테이블뷰의 행동과 관련된 내용이, UITableViewDataSource에는 테이블 뷰의 형태와 데이터에 관련된 내용들이 정의 되어있으니 꼭 한번 메소드 이름이라도 다 읽어보시기 바랍니다.


### 테이블 뷰의 필수요소
테이블 뷰를 생성하는 방법에는 2가지 방법이 있습니다. `init(frame: CGRect, style: UITableView.Style)
`를 써서 코드로 생설할 때 불리는 초기화 함수와 `init?(coder: NSCoder)`이라는 초기화 함수가 있습니다. 이 중 UITableView.Style은 `plain`, `grouped`, `insetGrouped` 세가지가 있습니다.


테이블 뷰를 사용할 때 설정할 수 있는 요소들은 다음과 같습니다. `style`, `tableHeaderView`, `tableFooterView`, `backgroundView` 들을 설정할 수 있습니다.

HeaderView와 FooterView의 설정 가능한 값들은 다음과 같습니다. `sectionHeaderHeight`, `sectionFooterHeight`, `estimatedSectionHeaderHeight`, `estimatedSectionFooterHeight`.


cell의 설정가능한 요소들은 다음과 같습니다. `rowHeight`, `estimatedRowHeight`, `cellLayoutMarginsFollowReadableWidth`, `insetsContentViewsToSafeArea`. 각각의 요소들이 의미하는 바는 좀 더 딥하게 정리할 때 다루도록 하겠습니다. 일단은 키워드들을 눈에 익히며, 아 이런 것들이 있었지를 위해 큰 아웃라인을 그리는데 집중하도록 하겠습니다.


dataSource를 지정하고, UITableViewDataSource 프로토콜을 채택해서 테이블 뷰의 데이터를 제공할 수 있습니다. 이 문서를 보면서 알게 되었는데, 데이터를 제공할 때 미리 제공해, 한 번에 많은 양의 데이터를 제공하면서 일어나는 버벅임을 막아주는 `UITableViewDataSourcePrefetching` 프로토콜을 채택하고 `prefetchDataSource`를 지정해서 좀 더 원할한 데이터를 제공할 수 있습니다. 이 부분에 대해서는 공부하고 따로 더 정리해 두겠습니다. 


재사용 가능한 셀은 `register`메소드를 이용해서 등록해주어야 사용이 가능합니다. Nib을 등록하거나, Class를 등록할 수 있습니다. 또한 `dequeueReusableCell` 메소드를 사용해서 해당하는 identifier를 가진 셀을 가져올 수 있습니다. 


tableView의 Section의 HeaderView와 FooterView를 `register`메소드를 이용해서 등록하고, `dequeueReusableHeaderFooterView`로 불러올 수 있습니다. 섹션의 셀이 아닌 헤더나 푸터에 들어가야 할 내용이 있다면 이 뷰들을 활용해야겠습니다.  


`delegate`를 설정 하고 `UITableViewDelegate` 프로토콜을 채택하므로써 테이블 뷰에 일어나는 이벤트들을 캐치할 수 있습니다.


셀을 구분하는 설정 값에는 다음과 같은 값들이 있습니다. `separatorStyle`으로는 한 줄로 구분 해주는 옵션과 아예 줄이 없는 옵션이 있습니다. `separatorColor` 구분하는 선의 색상입니다. `separatorEffect`, `separatorInset`, `separatorInsetReference`를 추가적으로 설정할 수 있습니다.


`numberOfRows`와 `numberOfSections`로 섹션의 갯수와 섹션별로 몇개의 셀을 그릴지를 결정할 수 있습니다.


섹션과 셀의 값을 가져오는 메소드는 다음과 같습니다. `cellForRow`, `headerView`, `footerView`, `indexPath`, `indexPathForRow`, `indexPathsForRows`, `visibleCells`, `indexPathsForVisibleRows`


row를 선택하는 방법에는 다음과 같은 방법들이 있습니다. `indexPathForSelectedRow`, `indexPathsForSelectedRows`, `selectRow`, `deselectRow`, `allowsSelection`, `allowsMultipleSelection`, `allowsSelectionDuringEditing`, `allowsMultipleSelectionDuringEditing`,`selectionFollowsFocus`, `selectionDidChangeNotification`

 
row와 section을 삽입하거나, 삭제, 이동하는 방법입니다. `insertRows`, `deleteRows`, `insertSections`, `deleteSections` `UITableView.RowAnimation`, `moveRow`, `moveSection` 


이번에 문서를 읽으면서, 처음 접한 부분입니다. 여러줄의 삽입, 업데이트, 삭제가 일어날 때 사용합니다. `performBatchUpdates`, `beginUpdates`, `endUpdates`.


섹션의 인덱스 관련 설정 값들 입니다. `sectionIndexMinimumDisplayRowCount`, `sectionIndexColor`, `sectionIndexBackgroundColor`, `sectionIndexTrackingBackgroundColor`


테이블 뷰의 재호출과 관련된 값들 입니다. `hasUncommittedUpdates`, `reloadData`, 
`reloadRows`, `reloadSections`, `reloadSectionIndexTitles`


드래깅과 관련된 값들 입니다. `dragDelegate`를 설정해줍니다. 그리고 `UITableViewDragDelegate` 프로토콜을 채택 해 줍니다. 그리고 나서 다음과 값들을 설정해줍니다. `hasActiveDrag`, `dragInteractionEnabled`


드롭과 관련된 값들 입니다. `dropDelegate`를 설정해줍니다. `UITableViewDropDelegate` 그리고 `hasActiveDrop` 값을 얻을 수 있습니다.


테이블 뷰의 스크롤링과 관련된 값들 입니다. `scrollToRow`, `scrollToNearestSelectedRow`, 
`UITableView.ScrollPosition`은 top, middle, bottom의 선택 값을 가집니다. 


테이블을 수정가능한 상태로 바꿉니다. `setEditing`, `isEditing`


테이블 뷰가 그려지는 영역의 값을 가져옵니다. `rect`, `rectForRow`, `rectForFooter`, `rectForHeader`


마지막으로 포커싱된 셀인지에 대한 여부를 가지고 있습니다. `remembersLastFocusedIndexPath`


### 정리

나열해 놓고 보니, 엄청나게 많은 내용들을 나열해 놓기만 했습니다. 해당 키워드 들이 보통 어떤 기능을 하는지, 어떤 값들과 같이 묶여있는지만 기억해 놓고 실제 필요할 때 더 찾아보는 방법으로 문제를 마주치면 해결하겠습니다. 각 파트에 대해서는 좀 더 자세하게 정리 해 두겠습니다. 



















