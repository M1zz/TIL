[Stopwatch](https://github.com/soapyigu/Swift-30-Projects/tree/master/Project%2002%20-%20Stopwatch)

### 앱 뜯어보기
#### 화면  
1. 절반은 뷰, 절반은 테이블 뷰
2. 뷰의 절반은 디스플레이, 나머지는 제어 버튼
3. 테이블 뷰는 기록 셀이 들어가야 함

#### 기능
1. 한 페이지 짜리 앱이다.
2. 초기 상태 : [LAB, START], START 버튼을 누르면 [LAB, STOP] LAB은 기록. STOP 누르면 [RESET, START]
3. 시작 버튼을 누르면 스톱워치가 가야한다.
4. 랩을 누르면, 기록이 저장되어야한다.

### 화면 구성
storyBoard로 구성할 수 있는 만큼 함.  
싱글 뷰를 하나 만들고 하위에 스탑워치를 위한 뷰, 시작 종료를 위한 버튼2개, 기록을 나타낼 테이블 뷰를 넣는다.  
뷰에는 라벨을 2개 넣어 기본 경과 시간과, 랩간 차이를 보여주는 라벨을 넣는다.  
조작 버튼은 누를 때 마다 스탑워치의 상태를 변경한다.  
테이블 뷰를 하나 만들고, 테이블 뷰 안에는 랩 버튼을 누를 때마다 셀을 넣어준다.

### 버튼동작 [스타트, 랩, 리셋, 스탑 버튼]
다른 기능은 생각하지 않고 버튼의 동작과, 스탑워치의 진행 상태값만 고려하여 구현한다.
- 초기는 [LAB, START] 상태로 둔다.
- START -> [LAB, STOP] -> [RESET, START]
- RESET 버튼을 누르면 초기화 상태.
- LAB 버튼을 누르면, 테이블에 시간 기록.

### 시간 시작, 멈춤, 초기화
- isPlaying 이 true인 상태 즉 기록이 되는 중 에만 시간이 가고 false인 상태에서는 멈춰야한다.
- 데이터를 위해 타이머 모델을 하나 만들어준다. 필요한 내용은 타이머객체와 시간카운터이다.
- 타이머는 [문서](https://developer.apple.com/documentation/foundation/timer/2091889-scheduledtimer)를 보고 만든다.
- 0.035 초마다 시계를 업데이트 해줄 것이고 계속 반복하기 때문 `mainStopwatch.timer = Timer.scheduledTimer(timeInterval: 0.035, target: self, selector: #selector(updateMainTimer), userInfo: nil, repeats: true)` 이렇게 만든다.  
- Stopwatch mainTimer를 만들어준다. 
- RunLoop.current.add를 사용해서 등록해주고 mainStopwatch.timer.invalidate()로 멈춰준다. [참고](https://www.silphion.net/2859)
- 같은 방법으로 labTimer를 만들어 준다.
- 초기화 버튼은 모든 타이머의 값을 00:00:00 로 입력해준다.

### LAB 버튼
LAB 버튼을 누르면 랩타이머가 초기화 되면서, 테이블에 메인타이머와 랩타이머의 값이 기록된다. 
- 랩 버튼을 눌렀을 때, 랩 타이머를 00:00:00 으로 초기화 해준다
- 테이블 뷰를 구현해준다. 테이블 뷰를 눌러서 반응을 하지 않으니 UITableViewDataSource 프로토콜만 채택해서 구현해준다. numberOfRowsInSection과, cellForRowAt를 구현해준다.
- 해당 뷰에 위임을 해준다.
- 버튼이 눌렸을 때의 mainTimer의 값을 저장하는 리스트에 값을 추가한다.
- labTimer도 저장해주는 리스트를 구현하자.
- 테이블을 리스트의 값으로 그린다. (by .reloadData()) 

### 리랙토링
isPlaying의 상태값을 기준으로 버튼 2개의 클릭이 되었을 때 동작해주면서 코드가 길어졌다.
```
@IBAction func playPauseTimer(_ sender: AnyObject) {
        if isPlaying {
            // stop
            ...
        } else {
            // start
            ...
        }
        
    }
@IBAction func lapResetTimer(_ sender: AnyObject) {
        if isPlaying {
            // Lab
            ...
        } else {
            // Reset
            ...
        }
    }
```
리팩토링 해야할 요소들을 찾아보았다.
1. 변수와 메소드의 위치, 주석달기
2. 같은 버튼의 색과 텍스트만 바꿔서 그리는 것
3. 타이머 초기화

완성!


[전체소스코드](https://github.com/M1zz/Stopwatch)
