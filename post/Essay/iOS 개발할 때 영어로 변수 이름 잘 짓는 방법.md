## iOS 개발할 때 영어로 변수 이름 잘 짓는 방법

개발을 하면서 개발자가 가장 많은 시간을 쓰는게 변수 명을 짓는 것 이라는 이야기가 있습니다.
이게 그냥 우스갯소리로 나온 것이 아니라, 제 개인적인 경험만 돌아봐도 개발시간의 많은 부분을 변수를 짓고 변수를 어떻게 지어야 하나 고민했습니다.

그 이유는 변수 명에 따라 그 이후의 개발 속도가 달라지기 때문입니다. 그래서 지금 짓는 변수에 많은 고민을 하고 시간을 소요하게 되었습니다.

글로는 알기 어려우니 예를 들어서 코드로 설명해 보겠습니다.

## 불명확한 이름이 가져오는 참사
틀리지는 않았지만 불명확한 이름이 가져오는 참사는 다음과 같습니다.

```
let numbers = [1,2,3,4,7]
let number = getSomeNumber(with: numbers)
```
함수의 이름이 `getSomeNumber` 이라서 어떤 숫자가 반환될지 알기가 어렵습니다. 그래서 우리는 이 함수가 어떻게 생겼는지 알아보러 가야합니다.

```
let numbers = [1,2,3,4,7]
let number = getSomeNumber(with: numbers)

func getSomeNumber(with numbers: [Int]) -> Int? {
  // ..... some code
  return numbers.first
}
```

이렇게 보았을 때, 함수가 어떻게 동작하는지 내가 분석을 하고 반환값을 보고 나서야 변수 이름을 지을 수 있습니다.

```
let numbers = [1,2,3,4,7]
let firstNumber = getSomeNumber(with: numbers)

func getSomeNumber(with numbers: [Int]) -> Int? {
  // ..... some code
  return numbers.first
}
```

## 영어 문법에 맞는 이름
변수 이름을 영어로 짓기 때문에, 문법에 맞지 않는 이름은 의미가 불명확하게 전달 될 수 있습니다.
그렇기 때문에 최소한 문법적으로 의미가 맞도록 지어주면 가독성이 좋아집니다.

### 단수, 복수
단수는 1개만 존재한다고 느껴지게하고, 복수는 여러개가 존재할 수 있음을 나타내줍니다.
물론 이는 컨벤션이고, 경우에 따라서는 s가 아닌 다른 복수의 의미를 나타내는 이름을 작성할 수 있습니다.
```
// preferred
let friend = "Leeo"
let friends = ["Leeo", "Olivia", "Dodo"]

// not preferred
let friend = ["Leeo", "Olivia", "Dodo"]
```

```
// preferred
let hasFriend = false
let haveFriends = false

// not preferred
let hasFriends = false
```
꼭 문법적으로 맞아야 하지는 않지만, 맞추면 가독성이 좋아진다는 뜻이니 이름을 짓기위해서 영어를 조금 더 공부하자는 것은
맞지만 영어부터 해야한다는 의미는 아닙니다!

### 분사
현재분사와 과거분사는 다른 느낌을 줍니다. 수동 능동의 의미도 있고 진행됨과 이미 고정된 느낌도 주죠.

```
// preferred
let selectedCell = UITableViewCell()
let showingCell = UITableViewCell()

// not preferred
let selectingCell = UITableViewCell()
```
보여지고 있는 셀인지, 선택된 셀인지에 대한 느낌을 더 잘 전달해줍니다.


### 형용사, 명사
is + 명사는 그것인지 아닌지를 판단하는 느낌입니다.
is + 형용사는 그러한 상태인지를 판단하는 느낌입니다.
현재분사와 과거분사를 적절히 사용해야 합니다.
보통 판단하는 형태의 변수명은 Bool 타입에 많이 이용됩니다.
```
// preferred
let isAdult = false
let isHidden = true
let hasShowing = true

// not preferred
let checkAdult = false
```

## 조동사와 동사원, 부사도 잊지 말기
문법에 맞추는 것은 실제로 변수명 보다는 함수에서 많이 이용됩니다.

```
let names = ["Leeo", "Olivia", "Dodo"]

// preferred
names.remove(at: 1)

// not preferred
names.remove(1) // 1을 지워라?
```

```
let names = ["Leeo", "Olivia", "Dodo"]

// preferred
names.insert("Ho", at: 0)

// not preferred
names.insert("Ho", 0)
```

## 정리

생각보다 변수명을 짓는제 제약도 많고, 고려해야 할 사항들도 많습니다. 금방금방 생각나는 변수명을
짓기 보다는 좀 더 고민해서, 미래의 나와 팀원들이 이해하기 좋은 코드를 짜는 방법을 고민해봅시다.

더 좋은 팁이나 정보를 알려주시면 추가 해놓도록 하겠습니다!
