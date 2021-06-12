이미지 서버에서 이미지를 가져다 표시해줄 때 항상 쓰던 라이브러리 입니다. 
어떤 기능이 있는지 살펴보기 전부터 웹에서 주소로 이미지를 가져올 때는 항상 써서, 
이미지를 불러오는 라이브러리 인 줄 알았습니다.

하지만 본래의 목적은 이미지 캐싱이라는 것을 알게 되었고, 이번기회에 어떤 기능이 있는지 살펴보겠습니다.

내용은 [Cheat Sheet](https://github.com/onevcat/Kingfisher/wiki/Cheat-Sheet)를 참고했습니다.


### 이미지 로딩
저도 이 기능 때문에 사용하는 줄 알았던 이미지 로딩입니다. 사용법은 굉장히 간단합니다. 
이미지 뷰를 포함해(NSImageView, UIButton and NSButton)에 `Extension`으로 구현되에 있기 때문에 `setImage`를 사용해 이미지를 설정하면 됩니다.

```
import Kingfisher

let url = URL(string: "https://example.com/image.png")
imageView.kf.setImage(with: url)
```

내부의 동작과정은 다음과 같습니다.

1. `url.absoluteString`로 캐싱된 이미지가 있는지 확입합니다.
2. 만약 이미지가 캐싱되어 있어서 찾으면(메모리, 디스크 중), imageView.image에 나타냅니다.
3. 만약 찾지 못한다면, url로 요청을 날려 이미지를 다운받습니다.
4. 다운받은 데이터를 UIImage object로 변환합니다.
5. 변환한 이미지를 메모리나 티스크에 캐싱합니다.
6. 이미지 뷰에 표시합니다.

### 이미지 다운로드
이미지뷰이 이미지를 설정할 때 캐싱이 되어있지 않으면 다운로드 합니다.
```
case .network(let resource):
    let downloader = options.downloader ?? self.downloader
    let task = downloader.downloadImage(
        with: resource.downloadURL, options: options, completionHandler: _cacheImage
    )
```
downloadURL 메소드를 좀 더 살펴보도록 하겠습니다. `URLRequest`을 래핑해서 만들었습니다. 다른 포스팅에서는 URLRequest, URLSession을 정리 
해보도록 하겠습니다.
```
// Creates default request.
var request = URLRequest(url: url, cachePolicy: .reloadIgnoringLocalCacheData, timeoutInterval: downloadTimeout)
request.httpShouldUsePipelining = requestsUsePipelining
```

### 프로세서 
다운로드에 성공하고 나면 이미지 프로세서가 프로세싱을 합니다.
```
case .success(let (data, response)):
    let processor = ImageDataProcessor(
        data: data, callbacks: callbacks, processingQueue: options.processingQueue)
```
프로세싱의 경우 여러가지 옵션이 있습니다. 이미지를 수정하거나, 다운 샘플링 그리고 필터를 입히는게 가능합니다.
```
Built-in processors

// Round corner
let processor = RoundCornerImageProcessor(cornerRadius: 20)

// Downsampling
let processor = DownsamplingImageProcessor(size: CGSize(width: 100, height: 100))

// Cropping
let processor = CroppingImageProcessor(size: CGSize(width: 100, height: 100), anchor: CGPoint(x: 0.5, y: 0.5))

// Blur
let processor = BlurImageProcessor(blurRadius: 5.0)

// Overlay with a color & fraction
let processor = OverlayImageProcessor(overlay: .red, fraction: 0.7)

// Tint with a color
let processor = TintImageProcessor(tint: .blue)

// Adjust color
let processor = ColorControlsProcessor(brightness: 1.0, contrast: 0.7, saturation: 1.1, inputEV: 0.7)

// Black & White
let processor = BlackWhiteProcessor()

// Blend (iOS)
let processor = BlendImageProcessor(blendMode: .darken, alpha: 1.0, backgroundColor: .lightGray)

// Compositing
let processor = CompositingImageProcessor(compositingOperation: .darken, alpha: 1.0, backgroundColor: .lightGray)

// Use the process in view extension methods.
imageView.kf.setImage(with: url, options: [.processor(processor)])
```

또한 `CIFilter`를 이용해 자신만의 필터를 만들 수도 있습니다.

### 캐싱

프로세싱을 마치면 캐싱을 합니다. 캐싱은 메모리와 디스크에 합니다. 다음과 같은 순서로 살펴보면 어떻게 구현되어있는지 보기 쉬웠습니다.  
```
setImage -> retrieveImage -> loadAndCacheImage -> cacheImage -> store -> memoryStorage.storeNoThrow -> storage.setObject
```

메모리 캐싱의 경우 NSCache를 사용하여 캐싱을 합니다. 키는 이미지의 url입니다.  
디스크 캐싱은 Data의 .write를 이용해서 저장합니다. 마찬가지로 캐싱의 키는 이미지의 url입니다. 키를 디렉토리로 해서 스토리지에 저장합니다.

### 정리
킹피셔의 이미지 캐싱로직과 순서, 그리고 구현내용을 살펴보았습니다. 하지만 이번에 킹피셔를 정리하면서 킹피셔를 쓰는 이유는 많은 이미지를 불러오고
화면에 뿌려줄 때 깜빡임이나 끊김없이 높은 성능을 보여주기 때문이구나 라는 것을 알았습니다. 이 것을 가능하게 하는 방법은 쓰레드 잘 사용했겠구나
라는 것을 추측할 수 있었습니다. 다음에 내용을 보강한다면, 어떻게 끊김없이 이미지 데이터를 잘 다룰 수 있는지 추가하겠습니다. 





