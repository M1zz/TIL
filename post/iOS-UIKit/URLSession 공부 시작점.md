iOS 개발 뿐 아니라 많은 서비스 들을 개발하다 보면 네트워크를 다루게 됩니다.

네트워크를 처음 접한 건 라이브러리를 사용해서 입니다. 그런데 구현체가 어떻게 생겼는지 모르고, 네트워크 통신 규약 문서를 볼 수 없는 상태로 사용만 하는 것은 문제가 생겼을 때 해결할 수 없었습니다. 그래서 `moya`나 `alamofire`과 같은 라이브러리들이 어떻게 구현되어 있는지 그리고 `URLSession`은 어떻게 이루어져 있는지 정리하려고 합니다.

## 동기 비동기

네트워크를 공부하기 전에 동기, 비동기를 먼저 공부하는 걸 추천 드립니다. 왜냐하면 네트워크는 여러가지 요청과 응답이 처리되어야 하는데, 이 작업들이 동기로 처리되어야 하는지 비 동기로 처리되어야 하는지를 판단하거나 이해하지 못하면 전반적인 내용에 대해서 이해하기 어렵습니다. 해당 내용은 [여기](https://dev200ok.blogspot.com/2020/08/blog-post_17.html)에 정리한 적이 있습니다.

## 네트워크 프로세스
가장 많이 만나는 코드 중 하나를 가지고 예를 들어 보도록 하겠습니다
```
let urlSession = URLSession(configuration: .default) // 1.

let task = urlSession.dataTask(with: url) { data,  response, error in // 2.
  print(response ?? "no response")
}

task.resume() // 3.
```

1. URLSession 클래스를 이용해 세션을 하나 만듭니다.
2. 그 세션은 여러가지 일을 할 수 있는데, URLSession에 구현되어있는 메소드로 어떤 일(task)을 추가할 수 있습니다. 그리고 그 일을 하는 task 객체를 반환 합니다.
3. task는 resume() 으로 실행합니다.

그럼 순서대로 알아보도록 하겠습니다.

## 세션 만들기

`URLSession`은 세션을 만든다고 언급했습니다. 그러면 어떻게 만들 수 있을까요? 코드를 살펴보면 `init(configuration: URLSessionConfiguration)` 이런 생성자로 세션을 만들 수 있습니다. 세션을 만들 때 URLSessionConfiguration 가 필요합니다.

- URLSessionConfiguration
  - default Session: 기본적인 세션입니다. 디스크 기반 캐싱을 지원합니다.
  - ephemeral Session: 어떠한 데이터도 저장하지 않는 세션입니다.
  - background Session: 앱이 종료된 이후에도 백드라운드에서 통신을 지원하는 세션입니다.

```
let urlSession = URLSession(configuration: .default)
```
(필수) 설정값을 넣어주어 urlSession을 하나 만들었습니다. 쉽죠? ㅎ ㅎ

## URLSessionTasks 만들기

URLSession은 URLSessionTask를 만들어 냅니다. session이 session task(할 일)을 만들어 내는 것이죠. 그렇게 만들 수 있는, 세션이 할 수 있는 Task는 크게 5가지입니다.

- URLSessionTask
  - URLSessionDataTask
  - URLSessionUploadTask
  - URLSessionDownloadTask
  - URLSessionWebSocketTask
  - URLSessionStreamTask


가장 일반적으로 접한 task를 만드는 코드는 아마 아래와 같은 코드 일 것 입니다.
아래 코드는 위에서 보여드린 세션을 만들어 요청을 보내는 코드가 아닌 URLSession의 싱글톤 객체를 이용헤 요청을 보내는 코드 입니다! (그래서 세션을 만드는 부분이 없어요!)
```
let task = URLSession.shared.dataTask(with: url) { data, response, error in
    //code
}
task.resume()
```
위 코드 `dataTask()`는 URLSessionDataTask를 반환합니다. 그렇게 만들어진 task를 resume() 해서, 만든 요청을 보내야 합니다.
그러면 어느정도의 시간이 흐른 후 (아마 빛의 속도겠죠?) 응답이 옵니다. 그 응답은, data, response, error에 차곡 차곡 쌓입니다.

그럼 이제 다른 Task에 대해 알아보겠습니다.

`class URLSessionDataTask : URLSessionTask` 공식문서를 살펴보면, NSData 타입의 객체를 반환해주는 task 입니다. 즉 서버에 데이터를 요청해서 받아오는 session task 입니다.

`class URLSessionUploadTask : URLSessionDataTask` 서버로 데이터를 업로드 하는 작업을 수행하는 session task 입니다.

`class URLSessionDownloadTask : URLSessionTask` 이 세션 task는 다운 받은 데이터를 파일로 저장하는 URL session task 입니다.

`class URLSessionWebSocketTask : URLSessionTask` WebSockets 프로토콜 표준을 통해 통신하는 URL session task 입니다.

`class URLSessionStreamTask : URLSessionTask` 스트림 기반 URL session task입니다.

## 요청 보내서 응답 받기
할 일을 다 만들고 나면, 이제 일을 시켜야죠. 아래의 한 줄 코드를 실행해서 요청을 보낼 수 있습니다.
```
task.resume()
```

우리는 위에서, 받은 응답을 data, response, error 에 저장 받을 것 이라고 지정 해 놓았으니, 이제 응답을 확인하고 받은 데이터를 검증해서 사용하면 되겠죠?

네트워크 응답처리에 대한 글을 다음에 한 번 더 정리하겠습니다.

## 정리
평소에 AF.request(url)과 같이 편하게 쓰던 내용들이 어떻게 구현되어 있는지 보려니, urlSession에 대한 이해가 필요했습니다. 그리고 urlSession을 사용해서 데이터를 받았음에도 그 구조를 이해하지 못 했는데 이번에 urlSession과 urlSessionTask에 대해 명확하게 정리할 수 있었습니다.
