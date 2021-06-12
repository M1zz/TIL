Model - 데이터에 관한 로직
View - 사용자에게 보여지는 화면
Controller - Model과 View 사이의 동작관리

사용자가 블로그 앱에 글을 작성했을 때, View가 변하게 된다. 이때 Controller는 View에서 입력된 값들을
모델로 전달 해준다. 만약 필요한 경우 Model에서 값을 계산해야 한다면, 결과값을 Controller에게 전달한다.
Controller는 전달 받은 값을 바탕으로 View를 업데이트 한다. MVC 디자인 패턴은 이렇게 역할에 따라 구조가 나뉜다.
UI를 바꾸고 싶으면 View를 수정하면되고, 데이터 처리 로직을 수정하고 싶으면 Model을 손보면 된다.

위에서 설명 했듯이 Model에서 View로 접근하는 일은 없다. 반대로 이야기 하면, Controller가 중간에서 Model에게 View가 보낸 값을
전달해주고, Model의 결과값을 View로 전달 해 업데이트 할 수 있게해야한다.  

1. View -> Controller : Delegate를 사용하여 해결(Controller가 View의 변경사항에 대응할 수 있음)
2. Model -> Controller : Notification을 사용하여 해결(Model의 값이 변경되었을 때 Controller에게 전달)
3. Controller -> View : 업데이트 한다.
4. Controller -> Model : 업데이트 한다.

역할을 단순하게 나누어 빠르게 구현할 수 있으나, View lifeCycle 코드가 Controller에 위치하기 때문에 Controller가 커진다.
