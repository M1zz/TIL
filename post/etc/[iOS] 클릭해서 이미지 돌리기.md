앱을 만들다가, `더 보기 ^`를 눌렀을 때 아래로 화살표가 돌아가면서 목록들이 나타나는 것을 구현할 일이 있었습니다. 
위로 향하는 이미지를 누르면 아래로 향하는 이미지로 바뀌도록 코딩을 하려고 했는데 이미지를 돌릴 수 있다는 것을 알고 정리해 두려고 합니다.

### 버튼으로 상하 반전하기
이 방법은 어떤 버튼을 눌렀을 때 이미지를 상하로 회전시키는 방법입니다.  CGAffineTransform을 이용하여 이미지를 뒤집습니다.
```
private var isUpside: Bool = false
@IBAction func turnArroundButtonClicked(_ sender: Any) {
    arrowImageView.transform = CGAffineTransform(scaleX: 1, y: isUpside ? 1 : -1)
    isUpside = !isUpside
}
```
이미지의 x좌표 y좌표를 변경해여 이미지가 돌아간 것 처럼 할 수 있었습니다. 원래 scaleX, Y로 이미지를 늘려주는 것 이지만,
y좌표를 음수를 할당 해 이미지에 반전을 주었습니다.

### 90도 만큼 돌리기
버튼을 눌러 이미지를 돌리다 보니, 시계방향, 반 시계방향으로 돌리고 싶을 때는 어떻게 해야하나 찾아보다가 발견한 방법입니다. 좋은 방법인지는 
잘 모르겠으니, 더 좋은 방법을 아시는 분은 댓글로 달아주세요!
```
private var count: Int = 0

@IBAction func clockWiseButtonClicked(_ sender: Any) {
    count += 1
    arrowImageView.transform = CGAffineTransform(rotationAngle: .pi * 0.5 * CGFloat(self.count))
    
}
@IBAction func reverseClockWiseButtonClicked(_ sender: Any) {
    count -= 1
    arrowImageView.transform = CGAffineTransform(rotationAngle: .pi * 0.5 * CGFloat(self.count))
}
```

### 버튼 눌러 돌리기
실제 제가 원했던 방법은 버튼을 눌러 이미지를 돌리는 것 이었습니다. 
1. 이미지 뷰를 클릭 했을 때의 이벤트 탐지할 수 있도록 설정
2. 이벤트가 발생했을 때, 이미지 회전 

```
override func viewDidLoad() {
    super.viewDidLoad()
    
    let tapGestureRecognizer = UITapGestureRecognizer(target: self, action: #selector(foldButtonClicked(tapGestureRecognizer:)))
    arrowImageView.isUserInteractionEnabled = true
    arrowImageView.addGestureRecognizer(tapGestureRecognizer)
}
```
뷰가 로드 되었을 때 tapGestureRecognizer를 추가 해줍니다. `foldButtonClicked` 매소드를 추가하고 구현해 주겠습니다.

```
@objc func foldButtonClicked(tapGestureRecognizer: UITapGestureRecognizer)
    {
        let tappedImage = tapGestureRecognizer.view as! UIImageView
        tappedImage.transform = CGAffineTransform(scaleX: 1, y: isUpside ? 1 : -1)
        isUpside = !isUpside
    }
```
우리가 추가한 제스쳐가 인식 될 때, 이미지를 위 아래로 반전시켜주는 기능을 하는 코드입니다.

 



전체 코드는 아래와 같습니다.
```
//
//  ViewController.swift
//  TurnAround
//
//  Created by 이현호 on 2020/09/29.
//

import UIKit

class ViewController: UIViewController {

    @IBOutlet weak var arrowImageView: UIImageView!
    @IBOutlet weak var turnAroundButton: UIButton!
    @IBOutlet weak var clockWiseButton: UIButton!
    @IBOutlet weak var reverseClockWiseButton: UIButton!
    
    private var count: Int = 0
    private var isUpside: Bool = false
    
    override func viewDidLoad() {
        super.viewDidLoad()
        
        let tapGestureRecognizer = UITapGestureRecognizer(target: self, action: #selector(foldButtonClicked(tapGestureRecognizer:)))
        arrowImageView.isUserInteractionEnabled = true
        arrowImageView.addGestureRecognizer(tapGestureRecognizer)
    }

    @IBAction func turnArroundButtonClicked(_ sender: Any) {
        arrowImageView.transform = CGAffineTransform(scaleX: 1, y: isUpside ? 1 : -1)
        isUpside = !isUpside
    }
    
    @IBAction func clockWiseButtonClicked(_ sender: Any) {
        count += 1
        arrowImageView.transform = CGAffineTransform(rotationAngle: .pi * 0.5 * CGFloat(self.count))
        
    }
    @IBAction func reverseClockWiseButtonClicked(_ sender: Any) {
        count -= 1
        arrowImageView.transform = CGAffineTransform(rotationAngle: .pi * 0.5 * CGFloat(self.count))
    }
    
    @objc func foldButtonClicked(tapGestureRecognizer: UITapGestureRecognizer)
    {
        let tappedImage = tapGestureRecognizer.view as! UIImageView
        tappedImage.transform = CGAffineTransform(scaleX: 1, y: isUpside ? 1 : -1)
        isUpside = !isUpside
    }
    
}

```








