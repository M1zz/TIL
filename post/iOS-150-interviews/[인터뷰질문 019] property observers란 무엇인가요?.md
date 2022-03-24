# [인터뷰질문 019] property observers란 무엇인가요?

관련주제 : Swift
난이도 : 하

## 프로퍼티와 옵져버
swift를 쓰다보면, 다양한 프로퍼티들이 있습니다. 쉽게 변수라고 알고있죠. 그리고 옵져버는 관찰자 혹은 감시자 정도로 번역할 수 있는데요. `property observers`를 직역해보면 변수 관찰자 정도가 될테고, 그 내용은 프로퍼티의 수정사항을 탐지하는 것 입니다. 즉 프로퍼티(저장 프로퍼티)에 변경사항이 생기면, 옵져버가 알려주고, 특정 코드블럭을 실행할 수 있습니다. 글로만 설명하면 어려우니 코드를 통해 알아보도록 하겠습니다.

```
class PointCounter {

    var totalPoint: Int = 0 {
        willSet(newTotalPoint) {
            print("점수의 합을 \(newTotalPoint)점으로 변경할 예정입니다.")
        }
        didSet(oldTotalPoint) {
            print("\(totalPoint - oldTotalPoint)점이 변경되었습니다.")
        }
    }
}

let pointCounter = PointCounter()

// 점수의 합을 200점으로 변경할 예정입니다.
pointCounter.totalPoint = 200
// 200점이 변경되었습니다.

// 점수의 합을 300로 변경할 예정입니다.
pointCounter.totalPoint = 300
// 100점이 변경되었습니다.

// 점수의 합을 200점으로 변경할 예정입니다.
pointCounter.totalPoint = 200
// -100점이 변경되었습니다.
```
## willSet didSet
이름부터 매우 직관적이죠? 이름에서 부터 알 수 있듯이 willSet은 프로퍼티가 변경되기 바로 직전에 호출 됩니다. 그리고 예상한 대로 didSet은 프로퍼티가 변경된 직후에 호출 됩니다.

그러니 우리가 `pointCounter.totalPoint`를 500으로 바꾼다면, willSet의 코드 블럭이 호출됩니다. 예상되는 결과는 `점수의 합을 500점으로 변경할 예정입니다.` 입니다. 그리고 500으로 수정된 후에는
`300점이 변경되었습니다.` 입니다.

## 정리
정리해보면, 프로퍼티 옵져버는 해당 프로퍼티의 변경사항을 관찰하는 감시자입니다. 우리는 그 감시자를 통해 프로퍼티의 변경 전이나, 변경 후에 필요한 코드를 실행할 수 있습니다.
