---
title: "[iOS/Swift] iOS13 에서 라이트모드(또는 다크모드)만 지원하기 (turn off darkmode)"
header:
  teaser: ""
categories:
  - iOS
  - ISSUENOTE
tags:
  - iOS
  - Swift
  - DarkMode
toc: true
---



iOS13부터 다크모드가 생겼습니다. <br>
그러면서 Xcode 디폴트가 다크모드/라이트모드 모두 지원 상태로 되어 하나만 지원하고 싶을 때 어려움을 겪었습니다. <br>


## 이번 글은 
iOS13에서 라이트모드(또는 다크모드)만 지원하기 위한 방법입니다. <br><br>

## 1. info.plist
```
<key>UIUserInterfaceStyle</key>
<string>Light</string>
```
info.plist에 해당 Source Code를 추가하거나
<br><br>


![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/210526/turnoff-darkmode-plist.png){: .align-center}
사진처럼 Appearance를 추가합니다.
<br><br>


다크모드만 지원하고 싶을 때는 Light -> Dark로 설정합니다.
{: .notice--primary}


## 2. AppDelegate.swift

AppDelegate.swift > didFinishLaunchingWithOptions에서 window 변수에 대해 아래와 같이 설정해도 됩니다.<br>
```swift
if #available(iOS 13.0, *) {
    self.window?.overrideUserInterfaceStyle = .light // 라이트모드만 지원하기
//    self.window?.overrideUserInterfaceStyle = .dark // 다크모드만 지원하기    
}
```
<br><br>

## 3. UIViewController

UIViewController마다 선택적으로 지원을 변경하고 싶다면 UIViewController 클래스 viewDidLoad 메서드 안에 작성합니다.<br>
```swift
override func viewDidLoad() {
    super.viewDidLoad()
    if #available(iOS 13.0, *) {
        overrideUserInterfaceStyle = .light // 라이트모드만 지원하기
//        overrideUserInterfaceStyle = .dark  // 다크모드만 지원하기
    }
}
```
<br><br>
