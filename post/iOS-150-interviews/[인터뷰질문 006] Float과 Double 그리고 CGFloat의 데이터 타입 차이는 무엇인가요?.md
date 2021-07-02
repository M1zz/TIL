# [인터뷰질문 006] Float과 Double 그리고 CGFloat의 데이터 타입 차이는 무엇인가요?

처음 숫자를 다룰 때는, `Int`형을 썼습니다. 그러다가 이제 나눗셈이 들어가면서 부터 실수를 사용해야 하는데,
그 때 마다 대충 적당히 쓰다가 정확한 차이점과 그 쓰임새를 정리하려고 합니다.

## Float vs Double
변수의 크기가 다릅니다. Int와의 다른점은 생략하겠습니다. 자세 한 부분은 [부동소수점](https://ko.wikipedia.org/wiki/부동소수점)을 참고하면 좋습니다.
Float은 32-bit, Double은 64-bit 입니다.

즉 Float은 2의 24승인 16777216.0, Double은 2의 53승인 9007199254740992까지 표현할 수 있습니다.
물론 정확한 범위는 소수점까지 포함되어야 하겠지만 Float과 Double의 크기 차이가 어느 정도 인지만 나타내겠습니다.

아래 코드는 궁금해서 출력해 본 코드 입니다. 궁금하신 분들은 한 번 복, 붙 해서 돌려보세요 :)

```
import Foundation

var float:Float = 16777216
var double: Double =  9007199254740992

print(float)
float = float + 1
print(float)

print(double)
double = double + 1
print(double)

var total:Double = 1
for i in 1..<55{
    total = total * 2
    print(i, total)
}
```

## Float vs CGFloat
CGFloat은 `import UIKit` 해야 쓸 수 있다는 점이 다릅니다. 돌아가는 기기에 따라 32-bit 이나 64-bit이 될 수 있으나,
현실적으로는 거의 64-bit입니다.

추가적으로 Swift 5.5 버젼 이상에서는 `CGFloat`과 `Double`이 교환 가능하다고 합니다.
