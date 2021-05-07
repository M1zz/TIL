Enumerations "열거" 임의의 관계를 맺는 값들을 하나의 타입으로 묶은 타입이다.



### 사용법



```

enum Fruits {

  case banana

  case apple

  case melon

  case tomato

}

```



### 타입지정

각각의 사례항목의 값을 지정하려면 타입을 지정하면 된다.



```

enum Fruits: String {

  case banana

  case apple

  case melon

  case tomato

}



// Fruits.banana.rawValue == "banana"

```



### 구분

Enum 이름이 다르기 때문에 같은 이름과 타입을 가졌어도 구분할 수 있다.



```

enum SomeEnums: Int{

  case one, two, three, four

}



enum AnotherEnums: Int {

  case one, two, three, four

}



var a: SomeEnums = .one // 타입이 분명하므로 헷갈릴 염려가 없다.

```



### 케이스 매칭

enum의 항목의 값을 구분해서 쓸 때는 주로 switch case문을 사용한다.

```

let myFavorite: Fruits = .banana

switch myFavorite {

case .banana

  print(.banana)

case .apple

  print(.apple)

case .melon

  print(.melon)

case .tomato

  print(.tomato)

}

```



### 연관값

연관값은 사례항목마다 각 타입값을 다르게 할 수 있다.



바코드를 정의하는 방법은 2가지가 있을 수 있다. 1차원과 2차원. 

enum을 사용하면, 이렇게 2종류의 타입을 정의 할 수 있다. 

```

enum Barcode {

    case upc(Int, Int, Int, Int)

    case qrCode(String)

}



var productBarcode = Barcode.upc(8, 85909, 51226, 3)



switch productBarcode {

case .upc(let numberSystem, let manufacturer, let product, let check):

    print("UPC: \(numberSystem), \(manufacturer), \(product), \(check).")

case .qrCode(let productCode):

    print("QR code: \(productCode).")

}

```



### Raw 값 (Raw Values) 

변하는 값이 아닌 하나의 값으로 채워질 수 있다.  

아래 코드는 아스키 코드값을 Raw Values로 저장하는 예 이다.

```

enum ASCIIControlCharacter: Character {

    case tab = "\t"

    case lineFeed = "\n"

    case carriageReturn = "\r"

}

```
