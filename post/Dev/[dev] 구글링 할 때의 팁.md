코딩을 하다가 에러메시지를 마주 했을 때 나에게 맞는 해결방법을 검색하는 방법의 팁에 대해 정리한다.

### `*` 사용하기
검색은 에러를 마주하면서 시작된다. 하지만 에러메시지를 그대로 복사 붙여넣기 해서는 나에게 필요한 질문과 답을 찾을 수 없다. 예를들면 `could not cast value of type 'Test.Home ViewController' to 'Test.List ListController'` 라는 에러메시지를 보고 해결하려고 전체를 복사 붙여넣기를 하면 이상한 결과가 나올 수 있다. 나의 코드에만 존재하는 키워드들이 들어있기 때문이다. `Test.Home`나 `ListController` 와 같은 것들이다. 그럴 때는 `*`를 활용해서 wildcard를 넣을 수 있다. 
다음과 같은 검색어가 된다. `could not cast value of type * to *`. 이렇게 하면 나의 코드에만 있는 부분을 포함해서 검색할 수 있다. 

### 특정 도메인의 활용
코드에서 발생한 대부분의 에러에 대한 질문과 답변을 stack overflow와 같은 곳에서 검색하고 싶다면, `site : [domain]`과 같은 키워드를 사용해보자.  
`site : stackoverflow.com could not cast value of type * to *`
이전 검색어가 다양한 곳에서의 질의 응답 결과를 보여줬다면, 이번 검색 결과는 stackoverflow의 검색 결과만 보여준다.

### "swift" 같은 추가적인 키워드 활용
stackoverflow에 있는 질의 응답 중에서도 "swift" 질문들만 보고 싶을 수 있다.
이럴 키워드를 하나 추가해주자.  `site : stackoverflow.com swift could not cast value of type * to *` 나에게 필요한 키워드 들을 추가함으로써 검색의 정확도를 올릴 수 있다.

### stackoverflow와 같은 도메인에 직접 들어가서 검색을 하는 것과의 차이
물론 직접 검색을 하는 방법도 있다. 하지만 검색 엔진에 따라 더 실망스러운 결과를 많이 접했다. 결과적으로 구글에 정보력을 믿고 구글에서 정확하게 검색하는 방법을 익히는 것이 더 효율적인 검색을 할 수 있다고 생각한다. 

참고 : https://www.avanderlee.com/optimization/developer-productivity-boost-with-google-search-tips-tricks/

