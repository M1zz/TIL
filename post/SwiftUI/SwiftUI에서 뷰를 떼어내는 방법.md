# SwiftUI에서 뷰를 떼어내는 방법

SwiftUI에서 UI를 그리다 보면, 뷰를 따로 떼어내서 작업을 해야하는 경우들이 발생합니다. 글로 설명해서는 상상하기 어려우니 아래 코드를 보면서 이야기 해보겠습니다.

```
struct ContentView: View {
    var body: some View {
        VStack {
            VStack {
                Text("리이오")
                Rectangle()
                    .fill(Color.red)
                    .frame(height: 30)
                Text("올리비아")

            }
            .background(.green)

            Button {
                print("done")
            } label: {
                Text("Button")
            }

        }
    }
}
```
우리의 코드는 VStack 안에 또다른 VStack과 Button이 있습니다. 저 사용자 이름이 들어있는 VStack이 복잡해질 경우 따로 떼어내서 작업을 하는 경우가 많습니다. 일단은 body가 길어지면 보기 힘들기 때문이죠. 이 때 사용하는 방법에는 여러가지가 있지만 오늘은 2개를 비교해 보겠습니다.

## 새로운 Struct를 만든다
새로운 뷰를 그리기 위해서 새로운 struct를 만들어서 그리는 방법이 있습니다. 이전 코드보다 보기가 한결 쉬워졌습니다. 또 ProfileView 부분이 늘어나도 ContentView는 늘어나지 않아서 가독성도 좋습니다.

```
struct ContentView: View {
    var body: some View {
        VStack {
            ProfileView()

            Button {
                print("done")
            } label: {
                Text("Button")
            }

        }
    }
}

struct ProfileView: View {
    var body: some View {
        VStack {
            Text("리이오")
            Rectangle()
                .fill(Color.red)
                .frame(height: 30)
            Text("올리비아")

        }
        .background(.green)
    }
}
```

## ViewBuilder를 이용한방법
`@ViewBuilder` 어노테이션을 붙여 뷰를 만들어 주는 메소드를 만들어 줄 수 있습니다. 위에서와 마찬가지로 코드의 가독성을 높여주며 다른 뷰를 독립적으로 보기 좋습니다.
```
struct ContentView: View {

    var body: some View {
        VStack {
            ProfileView()

            Button {
                print("done")
            } label: {
                Text("Button")
            }

        }
    }

    @ViewBuilder
    func ProfileView() -> some View {
        VStack {
            Text("리이오")
            Rectangle()
                .fill(Color.blue)
                .frame(height: 30)
            Text("올리비아")

        }
        .background(.green)
    }
}
```

## 언제 무엇을 쓰지?
두 코드의 화면은 다르지 않으며, 그 목적 또한 같아보입니다. 그러면 언제 어떤 방법을 써야할까요?

`@ViewBuilder`를 붙여서 만든다면 암시적으로 해당 뷰 안에서만들어 사용하겠다는 뜻 입니다. 즉 다른 파일에서 ProfileView를 그리지 않는다는 뜻 입니다.

반대로 struct로 ProfileView를 만든다는 것은 ContentView말고도 다른 View에서 ProfileView를 그릴 수 있음을 나타냅니다.

내가 그린 파일 안에서만 화면을 나누려면 `@ViewBuilder`를 사용하면 좋습니다. 그리고 다른 곳에서도 View를 그리려고 한다면 struct로 만들어서 작성하면 다른 사람들도 뷰를 그리기 좋겠죠?

물론 이렇게 해야만 하는 것은 아니지만 나의 의도가 조금이라도 더 담겨있다면 좋겠습니다.
