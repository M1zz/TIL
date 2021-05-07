[APNs를 이용해서 푸쉬](https://dev200ok.blogspot.com/2020/03/ios-notification-push.html)를 보낼 수 있다는 가정하게 작성된 글 입니다.

  

### Image Push Notification 만들기 (Using Notification Service Extension and Notification Content Extension).

[기본적인 알림 기능](https://dev200ok.blogspot.com/2020/03/ios-notification-push.html)이 동작하고 있다고 가정하고 설명합니다.

service extension과 content extension을 추가해줍니다.
Service extension은 Service라는 이름으로, Content extension은 Content라는 이름으로 추가 해 줍니다.
```
xcode > Editor > Add Target > Notification Service Extension > productname : "Service" > Next > Finish  
xcode > Editor > Add Target > Notification Content Extension > productname : "Content" > Next > Finish
```

[![](https://1.bp.blogspot.com/-PNPD1l8oqv8/XnIMwMihrFI/AAAAAAAAhIQ/4MESJT5U-74VfdaeD7kpND8TCaXetCPAgCLcBGAsYHQ/s400/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA%2B2020-03-18%2B%25E1%2584%258B%25E1%2585%25A9%25E1%2584%2592%25E1%2585%25AE%2B8.19.48.png)](https://1.bp.blogspot.com/-PNPD1l8oqv8/XnIMwMihrFI/AAAAAAAAhIQ/4MESJT5U-74VfdaeD7kpND8TCaXetCPAgCLcBGAsYHQ/s1600/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA%2B2020-03-18%2B%25E1%2584%258B%25E1%2585%25A9%25E1%2584%2592%25E1%2585%25AE%2B8.19.48.png)

추가 후 결과 화면은 다음과 같습니다.

Service > NotificationService.swift는 다음과 같습니다.
```
import UserNotifications

class NotificationService: UNNotificationServiceExtension {

    var contentHandler: ((UNNotificationContent) -> Void)?
    var bestAttemptContent: UNMutableNotificationContent?

    override func didReceive(_ request: UNNotificationRequest, withContentHandler contentHandler: @escaping (UNNotificationContent) -> Void) {
        self.contentHandler = contentHandler
        bestAttemptContent = (request.content.mutableCopy() as? UNMutableNotificationContent)
        
        if let bestAttemptContent = bestAttemptContent {
            // Modify the notification content here...
            bestAttemptContent.title = "\(bestAttemptContent.title) [modified]"
            
            var urlString:String? = nil
            if let urlImageString = request.content.userInfo["urlImageString"] as? String {
                urlString = urlImageString
            }
            
            if urlString != nil, let fileUrl = URL(string: urlString!) {
                print("fileUrl: \(fileUrl)")
                
                guard let imageData = NSData(contentsOf: fileUrl) else {
                    contentHandler(bestAttemptContent)
                    return
                }
                guard let attachment = UNNotificationAttachment.saveImageToDisk(fileIdentifier: "image.jpg", data: imageData, options: nil) else {
                    print("error in UNNotificationAttachment.saveImageToDisk()")
                    contentHandler(bestAttemptContent)
                    return
                }
                
                bestAttemptContent.attachments = [ attachment ]
            }
            
            contentHandler(bestAttemptContent)
        }
    }
    
    override func serviceExtensionTimeWillExpire() {
        // Called just before the extension will be terminated by the system.
        // Use this as an opportunity to deliver your "best attempt" at modified content, otherwise the original push payload will be used.
        if let contentHandler = contentHandler, let bestAttemptContent =  bestAttemptContent {
            contentHandler(bestAttemptContent)
        }
    }

}

@available(iOSApplicationExtension 10.0, *)
extension UNNotificationAttachment {
    
    static func saveImageToDisk(fileIdentifier: String, data: NSData, options: [NSObject : AnyObject]?) -> UNNotificationAttachment? {
        let fileManager = FileManager.default
        let folderName = ProcessInfo.processInfo.globallyUniqueString
        let folderURL = NSURL(fileURLWithPath: NSTemporaryDirectory()).appendingPathComponent(folderName, isDirectory: true)
        
        do {
            try fileManager.createDirectory(at: folderURL!, withIntermediateDirectories: true, attributes: nil)
            let fileURL = folderURL?.appendingPathComponent(fileIdentifier)
            try data.write(to: fileURL!, options: [])
            let attachment = try UNNotificationAttachment(identifier: fileIdentifier, url: fileURL!, options: options)
            return attachment
        } catch let error {
            print("error \(error)")
        }
        
        return nil
    }
}
```

추가 후 다음과 같은 바디를 전송합니다.
```
{  
   "aps":{  
      "alert":"flowers for Hyunho!",
      "badge":1,
      "sound":"default",
      "category":"CustomSamplePush",
      "mutable-content":"1"
   },
   "urlImageString":"https://res.cloudinary.com/demo/image/upload/sample.jpg"
}
```

  

[![](https://1.bp.blogspot.com/-hNjuIiwNMwM/XnINKomty5I/AAAAAAAAhIY/_VwdQT1UEb0hW2au8nSXHJY6gigfpIH3gCLcBGAsYHQ/s400/%25E1%2584%258B%25E1%2585%25B5%25E1%2584%2592%25E1%2585%25A7%25E1%2586%25AB%25E1%2584%2592%25E1%2585%25A9_20200304_165844_1.png)](https://1.bp.blogspot.com/-hNjuIiwNMwM/XnINKomty5I/AAAAAAAAhIY/_VwdQT1UEb0hW2au8nSXHJY6gigfpIH3gCLcBGAsYHQ/s1600/%25E1%2584%258B%25E1%2585%25B5%25E1%2584%2592%25E1%2585%25A7%25E1%2586%25AB%25E1%2584%2592%25E1%2585%25A9_20200304_165844_1.png)

[![](https://1.bp.blogspot.com/-pjmWG3bhep8/XnINK1RNoRI/AAAAAAAAhIc/7G0bbxF39-YILKFzLuvPqKhS2iE0WdiBACLcBGAsYHQ/s400/%25E1%2584%258B%25E1%2585%25B5%25E1%2584%2592%25E1%2585%25A7%25E1%2586%25AB%25E1%2584%2592%25E1%2585%25A9_20200304_165844_2.png)](https://1.bp.blogspot.com/-pjmWG3bhep8/XnINK1RNoRI/AAAAAAAAhIc/7G0bbxF39-YILKFzLuvPqKhS2iE0WdiBACLcBGAsYHQ/s1600/%25E1%2584%258B%25E1%2585%25B5%25E1%2584%2592%25E1%2585%25A7%25E1%2586%25AB%25E1%2584%2592%25E1%2585%25A9_20200304_165844_2.png)

  

  

  

참고 블로그 : [https://medium.com/@lucasgoesvalle/custom-push-notification-with-image-and-interactions-on-ios-swift-4-ffdbde1f457](https://medium.com/@lucasgoesvalle/custom-push-notification-with-image-and-interactions-on-ios-swift-4-ffdbde1f457)
