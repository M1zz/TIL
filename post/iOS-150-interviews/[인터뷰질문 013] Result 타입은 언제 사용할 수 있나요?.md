# [인터뷰질문 013] Result 타입은 언제 사용할 수 있나요?

Result 타입은 Swift5에 추가되었습니다. 그렇다면 왜 추가되어야 했는지 Result 타입이 없었을 때는 어떤 문제가 있는지 알아보면서 Result에 고마움에 대해 알아보도록 하겠습니다.

## Result가 없는 세상
Result 타입은 네트워크 요청 쪽에서 많이 사용되었습니다. 예를 들면 아래와 같은 상황에서 말이죠.
```
import Foundation

func getData<T: Decodable>(urlString: String, completion: @escaping (T?) -> Void) {
    let url = URL(string: urlString)!

    URLSession.shared.dataTask(with: url) { data, response, error in
        if let error = error {
            return completion(nil)
        }

        guard let data = data else {
            return completion(nil)
        }

        guard let response = response as? HTTPURLResponse, response.statusCode == 200 else {
            return completion(nil)
        }

        guard let parsedData = try? JSONDecoder().decode(T.self, from: data) else {
            return completion(nil)
        }

        completion(parsedData)
    }.resume()
}
```
`URLSession.shared.dataTask`에서 요청한 응답 중 에러를 처리하려면, 정상적인 케이스와 정상적이지 않은 케이스들을 구별해서 반환 해주어야 합니다.

그러면 사용할 때는,
```
getData(urlString: "api.com/getGameData") { (data: Data?) in
    if let data = data {
        print(data)
    }
}
```
이렇게 검사를 해서 사용합니다.

## 에러 메시지를 출력 하자
여기서 좀 더 발전하면 에러 메시지를 전달 해 줄 수 있습니다.

```
import Foundation

func getData<T: Decodable>(urlString: String, completion: @escaping (T?, String?) -> Void) {
    let url = URL(string: urlString)!

    URLSession.shared.dataTask(with: url) { data, response, error in
        if let error = error {
            return completion(nil,"errorMessage")
        }

        guard let data = data else {
            return completion(nil, "dataError")
        }

        guard let response = response as? HTTPURLResponse, response.statusCode == 200 else {
            return completion(nil, "responseError")
        }

        guard let parsedData = try? JSONDecoder().decode(T.self, from: data) else {
            return completion(nil, "ParseError")
        }

        completion(parsedData, nil)
    }.resume()
}
```
그리고 사용할 때는 이렇게 하죠.
```
getData(urlString: "api.com/getGameData") { (data: Data?, errorString) in
    if let errorString = errorString {
        print(errorString)
    }

    if let data = data {
        print(data)
    }
}
```

## 에러 메시지를 정의해보자
문자열로 반환하면, 의미에 따라 파악하기가 힘들고 오타에 대한 두려움도 사라지지 않습니다. 그렇기 때문에 에러를 미리 정의 해 놓는다면, 좀 더 편안한 코딩을 할 수 있겠죠!

```
enum ResponseError: Error {
    case invalidURL
    case dataError
    case reseponseError
    case parsingError
}

func getData<T: Decodable>(urlString: String, completion: @escaping (T?, ResponseError?) -> Void) {
    let url = URL(string: urlString)!

    URLSession.shared.dataTask(with: url) { data, response, error in
        if let error = error {
            return completion(nil, .invalidURL)
        }

        guard let data = data else {
            return completion(nil, .dataError)
        }

        guard let response = response as? HTTPURLResponse, response.statusCode == 200 else {
            return completion(nil, .reseponseError)
        }

        guard let parsedData = try? JSONDecoder().decode(T.self, from: data) else {
            return completion(nil, .parsingError)
        }

        completion(parsedData, nil)
    }.resume()
}
```

```
getData(urlString: "api.com/getGameData") { (data: Data?, error) in
    if let error = error {
        print(error)
    }

    if let data = data {
        print(data)
    }
}
```

## 아얘 실패와 성공의 케이스를 정의하자
```

enum ResponseResult<T> {
        case success(T)
        case failure(ResponseError)

    enum ResponseError: Error {
        case invalidURL
        case dataError
        case reseponseError
        case parsingError
    }
}

func getData<T: Decodable>(urlString: String, completion: @escaping (ResponseResult<T>) -> Void) {
    let url = URL(string: urlString)!

    URLSession.shared.dataTask(with: url) { data, response, error in
        if let error = error {
            return completion(.failure(.invalidURL))
        }

        guard let data = data else {
            return completion(.failure(.dataError))
        }

        guard let response = response as? HTTPURLResponse, response.statusCode == 200 else {
            return completion(.failure(.reseponseError))
        }

        guard let parsedData = try? JSONDecoder().decode(T.self, from: data) else {
            return completion(.failure(.parsingError))
        }

        completion(.success(parsedData))
    }.resume()
}
```
그리고 사용은 아래와 같이 간결해 질 것 입니다.

```
getData(urlString: "api.com/getGameData") { (result: ResponseResult<Game>) in
    switch result {
    case .success(let game):
        print(game)
    case .failure(let error):
        print(error)
    }
}
```

## 정리
결국 정리하면 Result는 성공과 실패 2가지 케이스를 가진 enum입니다. 에러를 정의하고 성공시 반환 할 데이터의 타입을 입력한다면 코드가 훨씬 간결해지는 걸 볼 수 있습니다.

우리는 모든 경우를 생각할 필요 없이, 성공과 실패가 각각 언제인지, 성공의 경우 무엇이 반환되는지만 집중해서 코드를 작성하면 됩니다.
