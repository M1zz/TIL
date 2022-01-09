#APIKit을 이해하고 살펴보기(작성중)

API로 요청을 보내고 응답을 받아야 할 일들이 많습니다. 잘 구조화 된 APIKit을 보면서 차용하려고 합니다.
공식적으로 제공하는 튜토리얼과 반대로 진행합니다. 왜 인지 모르지만 저는 그게 편해서...ㅎ ㅎ

## 요청 날리기
```
class func send<Request>(_ request: Request,
  callbackQueue: CallbackQueue? = nil,
  handler: @escaping (Result<Request.Response, SessionTaskError>) -> Void = { _ in }) -> SessionTask? where Request : Request
```
이렇게 무지막지하게 생겼지만, 결국 뜯어보면, `request`를 입력 받고, 그 결과를 입력받는 클로져가 `SessionTask?`로 반환하는 녀석입니다.

요약해보면, 요청한다음 그 결과를 받아서 처리하는 녀석입니다.

실제로는 아래와 같이 사용합니다.

```
let request = GetRateLimitRequest()

Session.send(request) { result in
    switch result {
    case .success(let rateLimit):
        print("count: \(rateLimit.count)")
        print("reset: \(rateLimit.resetDate)")

    case .failure(let error):
        print("error: \(error)")
    }
}
```
`request`를 받아서 Result의 `response`를 채워줍니다. 그러면 result에 `success`와 `failure` 중 하나가 올테니 응답을 처리 해주면 되겠죠?

## 요청 정의하기
그렇다면 우리가 날린 요청은 어떻게 생겼었을까요?
```
// https://developer.github.com/v3/rate_limit/
struct GetRateLimitRequest: GitHubRequest {
    typealias Response = RateLimit

    var method: HTTPMethod {
        return .get
    }

    var path: String {
        return "/rate_limit"
    }

    func response(from object: Any, urlResponse: HTTPURLResponse) throws -> Response {
        guard let dictionary = object as? [String: AnyObject],
              let rateLimit = RateLimit(dictionary: dictionary) else {
            throw ResponseError.unexpectedObject(object)
        }

        return rateLimit
    }
}
```
일단 우리가 구현해야 하는 요청은 API에 맞는 프로토콜을 채택합니다. 우리의 끝단을 구현하려면 그에 맞는 앞단을 골라야겠죠?
예를 들면 아래와 같은 API들이 있다고 가정하겠습니다.

- github.com/api
  - /user
  - /commit
  - rate_limit

- book.com/api
  - search
  - buy

rate_limit을 구현할 때는 `github.com/api`를 골라 구현해야합니다.

먼저 응답을 구현해줍니다. 응답 구현에 대한 내용은 다음 섹션에서 하겠습니다. 그 다음에 Request 프로콜에서 구현해야 하는 내용들을 구현합니다
