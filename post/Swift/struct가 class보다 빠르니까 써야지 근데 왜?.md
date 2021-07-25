# struct가 class보다 빠르니까 써야지 근데 왜?

`struct`와 `class`를 비교하면 항상 `struct`가 성능적으로 유리하기 때문에 쓰면 좋다고 합니다. 그런데 정작 왜 좋은거지? 라는 질문에 답할 수가 없어서 정리하게 되었습니다. 

그냥 카더라 라고 알고만 있는 것 보다는 궁금한 것에 대해 정확하게 알고 넘어가봅시다!

## 메모리 영역
`struct`는 `stack`영역에, `class`는 `heap`영역에 할당 됩니다. stack과 heap 의 차이점은 자료구조적인 문제로 검색해서 보고 오시면 좋을 것 같아요.
단순한 stack이 좀 더 복잡한 heap영역에 할당하고 해제하는 것 보다는 성능이 좋은게 당연해보입니다.

그러면 stack영역과 heap영역에 할당되는 기준은 무엇일까요? 간단하게 value type이면 stack 영역, reference type이면 heap 영역에 할당된다고 생가하시면 편할 것 같습니다.

## reference counting
`heap`영역에 메모리를 할당하다 보면, `reference counting`이 발생합니다. 이 영역에 할당된 메모리를 얼마나 참조하고 있는가 라는 숫자겠죠. stack영역에 할당된 메모리에는 reference count가 없습니다. 왜냐하면 value type의 데이터는 참조할 일이 없기 때문이죠.

메모리에 할당하고(할당을 했으니 reference count가 1이상이겠죠) 0이 되는 순간 메모리에서 해제 합니다. 만약 순환참조를 하거나, 사용이 완료되어 참조를 끊어 reference count가 줄어들다 보면 0이 되겠죠?

이렇듯 `reference count`를 관리해야 하기 때문에 추가적인 리소스가 사용됩니다.

물론 struct 내부에 reference type의 프로퍼티가 있다면, `reference counting`이 발생하겠죠?
