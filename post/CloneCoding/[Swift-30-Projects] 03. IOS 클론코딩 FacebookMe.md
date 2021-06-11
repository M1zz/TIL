테이블 뷰를 이용해서 화면을 구성해보자.

### 앱 뜯어보기
한 화면의 테이블 뷰이다. 상단에는 네비게이션 바가 있고 Facebook이라고 적혀있다.  
5개의 섹션으로 이루어져 있다. 프로필셀로 이루어진 섹션, 여러 정보들을 보여주는 섹션, 즐겨찾기 섹션, 설정과 정책 섹션, 로그아웃 섹션.  
페이지 이동 할 수 있는 부분에는 화살표가 있고, 로그아웃은 중앙에 정렬되어있다. 기능은 구현하지 않고 껍데기만 구현해본다.  

### 네비게이션 바
상단 네비게이션 바를 띄운다. 이번 프로젝트는 스토리보드를 쓰지 않고 진행한다. 하나의 테이블 뷰 이기 때문에 스토리보드를 쓰는 것 보다는 테이블 뷰를 보기 좋게 짜는게 구조화 되어 이해하기 쉬울 것 같다.  
**[스토리보드 없이 코딩하는 방법]()**을 이용해서 화면을 하나 띄운다. 다만 주의해야 할 점은 네비게이션 바 위에 뷰를 하나 띄운다는 점이다.
title을 설정해주고, `navigationController?.navigationBar.barTintColor`로 네비게이션 바의 색을 지정해주면 끝!

### 테이블 구조
전체 섹션은 5개이다.  
각 섹션별로는 1,7,1,3,1개의 로우가 있다.  
몇 몇 개의 로우를 제외하면, disclosureIndicator가 있다.  
테이블 셀은 눌리고 나면 선택해제가 바로 되어야 한다.  

### 프로필 섹션
프로필의 셀 모양은 subtitle이다. 다양한 [셀 모양](https://docs.microsoft.com/ko-kr/xamarin/ios/user-interface/controls/tables/customizing-table-appearance)을 알아두자.
텍스트라벨과, 이미지, 디테일텍스트라벨을 입력해주면 완성

### 기능 섹션
여러가지 기능이 있는 섹션이다. 셀의 스타일을 subtitle로 만들어주고 이미지와 타이틀을 넣어준다.

### FAVORITE 
헤더의 길이를 길게하기위해, 아무 내용도 없는 셀을 하나 추가해서 2배로 긴 헤더를 만들어 주었다.

### 설정 섹션
기능 섹션과 마찬가지로 이미지와, 내용을 넣어준다. 

### 로그아웃 섹션
글의 색상을 red로 바꿔주고, 정렬을 중앙으로 해준다.


### 리팩토링
테이블 뷰만 구현하는 프로젝트이기에 일단 돌아가게 만든다. 하지만 아직 Model도 만들지 않았고, 반복되는 코드들도 많기 때문에 리팩토링을 해준다.

### 모델 생성
사용자 정보 기반으로 만들어지는 부분이 있기 때문에 사용자 모델을 만들어준다.

### 고정 값 몰아넣기
실수를 막기 위해 "문자열"값을 모두 변수 값으로 빼준다.  
이미지의 파일 이름과 색상값들을 한 파일에 설정해준다.(Specs.swift)

### 셀 만들기
테이블 뷰에 쓰일 셀을 만들어준다. 계속 반복되어 쓰기 때문에 셀을 빼서 반복되는 코드를 줄여준다.

### 데이터 형태
TableKey를 만들어준다. section과 row에 따라 접근할 수 있게 만들어야 한다.   
단계별로 생각 해 보면 아래와 같다.
```
[
  [section: "value", row: "value"], // 한 줄에 한 섹션
  [section: "value", row: "value"],
  [section: "value", row: "value"],
]
```


```
[
  [section: "value", row: 
    [
      [image:"value",title:"value"]
    ], // 한 줄에 한 섹션
  [section: "value", row: "value"],
  [section: "value", row: "value"],
]
```

이렇게 된 데이터의 구조를 가지게 된다.
최종 목표는 `data[section]['row'skey']`로 각 섹션의 각 로우에 접근 할 수 있도록 하는 것이 목표이다.

### 테이블 뷰에서 그리기
indexPath에는 section값과, indexPath값이 있다. 섹션값은 위에서 부터 몇번째 섹션인지에 대한 위치값과 인덱스 패스는 각 섹션별로 몇번 째 줄에 데이터인지를 나타내준다.  

```
let modelForRow = rowModel(at: indexPath)

...

// 섹션을 입력 -> data의 한 섹션의 데이터 반환
private func rows(at section: Int) -> [Any] {
    return tableViewDataSource[section][TableKeys.Rows] as! [Any]
}

// IndexPath 입력 -> 섹션의 row 데이터 반환
private func rowModel(at indexPath: IndexPath) -> RowModel {
    return rows(at: indexPath.section)[indexPath.row] as! RowModel
}
```
테이블 뷰에서는 인덱스 패스를 넘겨, 섹션값과 로우에 설정된 값을 가져온다. 

케이스 별로 프로필인지, 이미지가 없는지에 따라 다르게 그려준다. 또한 섹션값이 있는지, 로그아웃인지에 따라 다르게 그려준다.

섹션의 갯수, 각 섹션별로 로우의 갯수는 미리 정의 해 놓은 데이터의 형태에서 추출해서 전달한다.  

마지막으로 프로필 셀의 높이를 지정해준다. 테이블 키가 없다면 0으로 해주고, 프로필일 경우 크게, 내용은 작게 지정해준다.




