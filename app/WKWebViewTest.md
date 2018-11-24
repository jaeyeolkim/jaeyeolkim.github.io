## iOS WKWebView 띄워보기

* xcode 실행 > iOS > Application > Single View App > next
* Product Name 에 앱 이름(testWeb) 입력 후 create
* testWeb 하위에 ViewController.swift 파일 클릭
* https://developer.apple.com/documentation/webkit/wkwebview 경로에 있는 내용을 복사하여 넣어준다

```
import UIKit
import WebKit
class ViewController: UIViewController, WKUIDelegate {
    
    var webView: WKWebView!
    
    override func loadView() {
        let webConfiguration = WKWebViewConfiguration()
        webView = WKWebView(frame: .zero, configuration: webConfiguration)
        webView.uiDelegate = self
        view = webView
    }
    override func viewDidLoad() {
        super.viewDidLoad()
        
        let myURL = URL(string:"https://www.apple.com")
        let myRequest = URLRequest(url: myURL!)
        webView.load(myRequest)
    }
}
```

* 빌드 하면 simulator 에 애플 홈페이지가 잘 열린다.
* 하지만 다른 url을 열려고 하면 오류가 발생한다.
* testWeb > info.plist 파일 선택 후 마우스 우측 버튼 > open as > source code 선택하여 <dict> 계층에 아래 내용을 넣어준다.
```
    <key>NSAppTransportSecurity</key>
    <dict>
        <key>NSAllowsArbitraryLoads</key>
        <true/>
    </dict>
```
* 다시 빌드하면 잘 된다.(http://localhost:8080 도 ok)
