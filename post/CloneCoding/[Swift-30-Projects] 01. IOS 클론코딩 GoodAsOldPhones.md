
[Good As Old Phones](https://github.com/soapyigu/Swift-30-Projects/tree/master/Project%2001%20-%20GoodAsOldPhones)

- 앱을 구조 살피기
- 앱 레이아웃 구성
  - Products
    - Details
  - Contact Us

### 앱을 구조 살피기
화면은 크게 2개의 Tab Bar로 구성되어있다. Products 와 Contact Us.
상품의 목록을 나열하는 Products는 테이블 뷰로 이루어져있다. 각각의 상품을 입력하면, 상세페이지로 넘어간다. 상세페이지에는 카트에 추가할 수 있는 버튼이 있다.
Contact Us 페이지는 하나의 UIView이며, 연락처가 적혀있다.

### 앱 레이아웃 구성
앱 동선은 Storyboard를 이용해서 구성해준다.
먼저 크게 Tab Bar 컨트롤러로 두 화면을 갈라준다. 그리고 각각의 Tab Bar의 Item의 이름을 Products와 Contact Us로 변경해준다.
간단한 Contact Us 페이지에는 UIView를 넣어준다.
Products 페이지에는 Navigation Controller와 TableView를 추가해준다.
TableView의 Cell 선택시에는 UIView로 이동할 수 있도록 해준다.

### Contact Us
간단한 페이지 부터 구현한다.(개인취향이기 때문이다.)
Custom Class로 ContactUsViewController를 추가해주고 하나 생성해준다.

```
override func viewDidLoad() {
    super.viewDidLoad()

    setupScrollView()
    setupContents()
}
```
ContactUsViewController를 만들어준다. Storyboard에서 해당 UIView를 컨트롤 할 컨트롤러로 UIViewController를 지정해준다.
뷰가 로드되는 시점에 스크롤 뷰를 하나 생성하고, 뷰를 하나 생성한다.
뷰에 스크롤뷰를 추가하고, 스크롤 뷰에 뷰를 서브뷰로 추가한다.
```
func setupScrollView() {
    scrollView.translatesAutoresizingMaskIntoConstraints = false
    contentView.translatesAutoresizingMaskIntoConstraints = false

    view.addSubview(scrollView)
    scrollView.addSubview(contentView)

    setScrollViewConstraints()
    setContentViewConstraints()
}
```
서브 뷰에 필요한 요소들을 추가해준다.

```
func setupContents() {
    scrollView.addSubview(aboutUsHeaderImageView)
    scrollView.addSubview(aboutUsTitleLabel)
    scrollView.addSubview(aboutUsDescriptionLabel)
    scrollView.addSubview(contactTitleLabel)
    scrollView.addSubview(contactEmailImageView)
    scrollView.addSubview(contactEmailLabel)
    scrollView.addSubview(contactPhoneImageView)
    scrollView.addSubview(contactPhoneLabel)
    scrollView.addSubview(contactWebImageView)
    scrollView.addSubview(contactWebLabel)

    configureAboutUsHeaderImageView()
    configureAboutUsTitleLabel()
    configureAboutUsDescriptionLabel()
    configureContactTitleLabel()
    configureContactEmailImageView()
    configureContactEmailLabel()
    configureContactPhoneImageView()
    configureContactPhoneLabel()
    configureContactWebImageView()
    configureContactWebLabel()

    setAboutUsHeaderImageViewConstraints()
    setAboutUsTitleLabelConstraints()
    setAboutUsDescriptionLabelConstraints()
    setAboutUsContactTitleLabelConstraints()
    setAboutUsContactEmailImageViewConstraints()
    setAboutUsContactEmailLabelConstraints()
    setAboutUsContactPhoneImageViewConstraints()
    setAboutUsContactPhoneLabelConstraints()
    setAboutUsContactWebImageViewConstraints()
    setAboutUsContactWebLabelConstraints()
}
```
서브 뷰로 추가해준 요소들을 설정 해주고 제약을 걸어준다.
```
func configureAboutUsHeaderImageView() {
    let headerImage: UIImage = UIImage(named: "header-contact")!
    aboutUsHeaderImageView.image = headerImage
    aboutUsHeaderImageView.sizeToFit()
}

func setAboutUsHeaderImageViewConstraints() {
    aboutUsHeaderImageView.translatesAutoresizingMaskIntoConstraints = false
    aboutUsHeaderImageView.centerXAnchor.constraint(equalTo: contentView.centerXAnchor).isActive = true
    aboutUsHeaderImageView.topAnchor.constraint(equalTo:  contentView.safeAreaLayoutGuide.topAnchor).isActive = true
    aboutUsHeaderImageView.widthAnchor.constraint(equalTo: contentView.widthAnchor).isActive = true
}
```

전체 뷰를 만드는 것은 엄청난 노가다였고, 오래 걸리긴 했지만 어렵진 않았다. 하지만 여기서 한가지 노가다를 했던 부분은 **UISrollView**설정 부분이었다. 아직도 완벽하게 이해하지는 못했지만, scrollView 위에  하나의 뷰를 더 얹어주고 그 UIView에 요소들을 박아줘야 스크롤이 움직였다. 또 하나 scrollView의 컨텐츠의 크기를 계산할 수 있도록 topAnchor와 bottomAnchor가 꼭 필요했다.

기회가 된다면 다른 방법으로 다시 scrollView를 이용해 화면을 구현해보겠다.


### ProductsTableView
네이게이션 뷰에 테이블 뷰를 하나 만들어주고, 배경색을 조금 어둡게 설정해준다. style을 grouped로 설정해준다. 뭔지는 모르지만 셀이 시작될 때 조금 여백이 생긴다. 공부해야겠다.
셀을 클릭하고 Accessory로 Disclosure Indicator을 넣어준다. 누르면 상세페이지로 이동할 것 같은 버튼이 생긴다.

먼저 데이터를 받아올 수 없으니 static 하게 넣어주려한다. 하지만 MVC디자인 패턴으로 구현하기 위해, Model을 정의해준다.
```
struct Product {
    var name: String?
    var cellImageName: String?
    var fullscreenImageName: String?

    init(name: String, cellImageName: String, fullscreenImageName: String) {
        self.name = name
        self.cellImageName = cellImageName
        self.fullscreenImageName = fullscreenImageName
    }
}
```
필요한건 상품의 이름, 상품의 사진, 상세 페이지에 들어갔을 때 나올 배경이다. 모델을 정의 했으니 데이터를 만들어 준다.
```
private func dataFetch() -> [Product]? {
        return [Product(name: "1907 Wall Set", cellImageName: "image-cell1", fullscreenImageName: "phone-fullscreen1"),
        Product(name: "1921 Dial Phone", cellImageName: "image-cell2", fullscreenImageName: "phone-fullscreen2"),
        Product(name: "1937 Desk Set", cellImageName: "image-cell3", fullscreenImageName: "phone-fullscreen3"),
        Product(name: "1984 Moto Portable", cellImageName: "image-cell4", fullscreenImageName: "phone-fullscreen4")]
    }
```
이제 테이블을 그리기 위해 꼭 필요한 UITableViewController를 구현한다. numberOfRowsInSection와 cellForRowAt
```
override func tableView(_ tableView: UITableView, numberOfRowsInSection section: Int) -> Int {
    return products?.count ?? 0
}

override func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell {
    let cell = tableView.dequeueReusableCell(withIdentifier: Cells.identifier, for: indexPath)
    guard let products = products else { return cell }

    if let imageName = products[indexPath.row].cellImageName {
        cell.imageView?.image = UIImage(named: imageName)
    }
    cell.textLabel?.text = products[indexPath.row].name

    return cell
}
```
이미지가 커서 테이블의 높이를 조정 해주었다.
```
override func tableView(_ tableView: UITableView, heightForRowAt indexPath: IndexPath) -> CGFloat {
    return 94
}
```

### ProductView
테이블 뷰에서 상품을 눌렀을 때 상세 페이지를 보여주어야 한다.
일단 새로운 ViewController를 만들어서 Storyboard의 화면과 연결해준다.
필요한것은, 이미지, 라벨, 버튼이므로 각각의 요소를 뷰에 추가해준다.
```
override func viewDidLoad() {
    super.viewDidLoad()

    setupContents()

}

private func setupContents() {
    view.addSubview(productImageView)
    configureProductImageView()
    setProductImageViewConstraints()

    view.addSubview(productNameLabel)
    configureProductNameLabel()
    setProductProductNameLabelConstraints()

    view.addSubview(productAddCartButton)
    configureProductAddCartButton()
    setProductProductAddCartButtonConstraints()

}
```

하지만 이 이후에 이전 테이블 셀을 클릭했을 때 클린된 셀의 정보를 뷰가 넘겨 받아야 한다. prepare라는 함수를 활용하면, 데이터를 다른 뷰에 전달 할 수 있다. Storyboard에서 'Push segue'부분을 눌러 identifier를 설정해주고, 아래와 같은 코드를 tableView에 작성해준다.
```
// ProductsTableViewController.swift
// MARK: - View Transfer
    override func prepare(for segue: UIStoryboardSegue, sender: Any?) {
        if segue.identifier == Cells.segueIdentifier {
        if let cell = sender as? UITableViewCell,
          let indexPath = tableView.indexPath(for: cell),
          let productVC = segue.destination as? ProductViewController {
          productVC.product = products?[indexPath.row]
        }
      }
    }
```
화면 구성은 끝났다. 카트에 담는 버튼을 클릭 했을 때 이벤트를 탐지하고, "버튼 클릭"을 출력해주면 완성.

```
private func configureProductAddCartButton() {
    ...
    productAddCartButton.addTarget(self, action: #selector(addToCartButtonClick), for: .touchUpInside)
}

@objc
private func addToCartButtonClick() {
    print("\(productNameLabel.text!) is tapped")
}
```
addTarget으로 어떤 메소드를 호출할 것인지 추가해주고 메소드를 구현해주면 된다.

완성된 소스코드는 아래에 남겨놓습니다.

[전체소스코드](https://github.com/M1zz/GoodAsOldPhones)

