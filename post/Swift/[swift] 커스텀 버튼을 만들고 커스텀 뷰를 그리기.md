


버튼영역 내에 그림을 그려 커스텀 버튼을 만들고,
UIView에 그림을 그려주는 기능을 구현하려합니다.

삼각형 버튼, 원버튼, 정사각형 버튼을 만들고, 각각의 버튼이 눌렸을 때 뷰의 그림이 버튼의 모양과 같이 나와야 합니다.

### 구조
메인 뷰 컨트롤러, 도형이 그려진 뷰, 버튼 3개
스토리 보드에는 버튼 개를 올려놓고, 라벨을 그리고, 그 아래 커스텀 뷰를 그려줍니다.

### 도형 그리기
새 파일을 만들어서 UIButton을 만들어 줍니다. 
class 앞에 `@IBDesignable`을 붙이면, 그린 결과가 storyBoard에 바로 적용됩니다.

```
//
//  CircleButton.swift
//  viewTest
//
//  Created by 이현호 on 2020/07/20.
//  Copyright © 2020 tempYsoup. All rights reserved.
//

import UIKit

@IBDesignable class CircleButton: UIButton {
    var shape:Shape = .circle
    
    override func draw(_ rect: CGRect) {
      let path = UIBezierPath(ovalIn: rect)
      UIColor.blue.setFill()
      path.fill()
    }
}
```

그리고, draw라는 함수를 override해 주면 커스텀 하게 그릴 수 있습니다.

위와 같이 삼각형, 정사각형도 그려줍니다.
스토리 보드에 배치 해 주었던 버튼의 클래스로 설정으 해주시면 짜잔 그림이 그려집니다.

### 클릭 이벤트
뷰 컨트롤러에 `@IBAction` 버튼 클릭을 클릭한 버튼을 인식해 label의 text를 변화시켜 줍니다.
```
enum Shape :String {
    case triangle
    case square
    case circle
}

@IBAction func clickCircle(_ sender: Any) {
    print("clickCircle")
    shapeLabel.text = Shape.circle.rawValue
}
```
이렇게 각각의 버튼에 대해 취해야 할 행동을 정의 해줍니다

### 커스텀 뷰 그리기
스토리 보드에 추가 해 놓은 커스텀 뷰를 위한 뷰에 그림을 그려봅시다. 앞에서 버튼과 같이, `@IBDesignable class CustomView: UIView`로 클래스를 만들어 줍니다.

뷰에서 사용될 변수를 하나 선언 해주고, 변수의 값에 따라 다르게 그리도록 로직을 구현해줍니다. 
```
//
//  CustomView.swift
//  viewTest
//
//  Created by 이현호 on 2020/07/20.
//  Copyright © 2020 tempYsoup. All rights reserved.
//

import UIKit

@IBDesignable class CustomView: UIView {
    var shape: Shape = .triangle {
      didSet {
            setNeedsDisplay()
        }
    }

    override func draw(_ rect: CGRect) {
        if shape == .triangle {
            let path = UIBezierPath()
            path.move(to: CGPoint(x: self.frame.width/2, y: 0))
            path.addLine(to: CGPoint(x: 0, y: self.frame.height))
            path.addLine(to: CGPoint(x: self.frame.width, y: self.frame.height))
            UIColor.green.setFill()
            path.fill()
        }
        if shape == .circle {
            let path = UIBezierPath(ovalIn: rect)
            UIColor.blue.setFill()
            path.fill()
        }
        if shape == .square {
            let path = UIBezierPath()
            path.move(to: CGPoint(x: 0, y: 0))
            path.addLine(to: CGPoint(x: self.frame.width, y: 0))
            path.addLine(to: CGPoint(x: self.frame.width, y: self.frame.height))
            path.addLine(to: CGPoint(x: 0, y: self.frame.height))
            UIColor.yellow.setFill()
            path.fill()
        }
    }

}
``` 

didset을 설정해주어, 뷰의 상태가 바뀔 때, 어떤 행동을 취할지 정의 해줍니다. 우리는 뷰에 그림을 다시 그리려 하는데, 아래에 그린 draw를 호출하는게 아니라 `setNeedsDisplay()`를 호출해 주어야 redraw합니다.

### 정리
처음에는 커스텀 뷰와 커스텀 버튼을 어떻게 그려야 할지 막막했습니다.  
일단 스토리보드에 버튼과 뷰를 그려주고 하나씩 연결해 나갔습니다.  
[도형그리기](https://zeddios.tistory.com/822)도 공부하고, [didset](https://medium.com/ios-development-with-swift/%ED%94%84%EB%A1%9C%ED%8D%BC%ED%8B%B0-get-set-didset-willset-in-ios-a8f2d4da5514)도 공부하면서 하나하나 그려나갔습니다.


[전체 소스](https://github.com/M1zz/customButtons)

![customView](https://github.com/M1zz/customButtons/raw/master/customView.gif)

