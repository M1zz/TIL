내가 원하는 커스텀 뷰를 만들다보면, UIView와 같은 기존의 뷰를 상속받아서 테두리나 투명도를 조절하는 등의 커스터마이징을 할 수 있습니다.

하지만 항상 개발하면서, 생성자가 헷갈려서 정리 해 놓도록 하겠습니다.

### 필수 생성자
- init(frame: CGRect)  
코드로 뷰를 만들면 호출됩니다. CGRect 타입으로 만들어 주어야 합니다.
  - 코드 상에서는 `let avatarImageView = GFAvatarImageView(frame: .zero)` 이렇게 선언 합니다.
  - 스토리 보드가 없을 때 사용합니다.
  
- init(coder: NSCoder)
스토리 보드에서 나오면서 UIView를 만들 때 사용되는 생성자 입니다. 스토리 보드에 그려놓으면 이 생성자가 호출됩니다. 
  - 코드 상에서는 `@IBOutlet weak var appIconImageView: SoftBorderImageView!` 이렇게 선언해줍니다.
  - 스토리 보드에 연결 되어있을 때 사용합니다.


### awakeFromNib
TableViewCell을 xib으로 만들 때 `override func awakeFromNib()`이 생기는데요, 이 awakeFromNib은 인스턴스 화 된 후 호출 됩니다. 
즉 init(coder: NSCoder) -> awakeFromNib() 이 호출됩니다.

헷갈리는 부분을 여러번 실행하고 애플 문서를 찾아 정리 해 보았는데요, 혹시 틀린 부분있으면 망설임없이 지적 부탁드립니다!
