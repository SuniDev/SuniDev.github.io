---
title: "[iOS/Swift] 외부 브라우저(사파리)로 링크 열기"
header:
  teaser: ""
categories:
  - iOS
  - ISSUENOTE
tags:
  - iOS
  - Swift
  - Utils
toc: true
---

## 이번 글은 
외부 브라우저(사파리)로 링크를 여는 방법입니다.<br><br>

## 1. UIApplication.shared.open()

```swift 
func openExternalLink(urlStr: String, _ handler:(() -> Void)? = nil) {
    guard let url = URL(string: urlStr) else {
        return
    }
    
    if #available(iOS 10.0, *) {
        UIApplication.shared.open(url, options: [:]) { (result) in
            handler?()
        }
        
    } else {
        UIApplication.shared.openURL(url)
        handler?()
    }
}
```
사파리로 링크를 여는 함수입니다.
<br><br>

## 2. 사용 예시

```swift 
import UIKit

class Utils {

    /**
     # openExternalLink
     - Parameters:
     - urlStr : String 타입 링크
     - handler : Completion Handler
     - Note: 외부 브라우저로 링크 오픈
     */
    static func openExternalLink(urlStr: String, _ handler:(() -> Void)? = nil) {
        guard let url = URL(string: urlStr) else {
            return
        }
        
        if #available(iOS 10.0, *) {
            UIApplication.shared.open(url, options: [:]) { (result) in
                handler?()
            }
            
        } else {
            UIApplication.shared.openURL(url)
            handler?()
        }
    }
}
```
공통적으로 사용하는 변수 및 함수들을 Utils 라는 공통 클래스를 생성하여 관리하는 예시입니다. <br>

```swift
Utils.openExternalLink(urlStr: "https://www.google.co.kr/")
```
필요한 곳 어디서든 생성한 함수를 호출하여 사용합니다.<br><br>
