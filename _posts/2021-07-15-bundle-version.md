---
title: "[iOS/Swift] 현재 번들 버전(Bundle Version) 구하기"
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
현재 APP의 번들 버전(Bundle Version)을 구하는 방법입니다.<br><br>

## 1. CFBundleShortVersionString

```swift 
Bundle.main.infoDictionary!["CFBundleShortVersionString"] as! String
```
현재 APP의 번들 버전을 구하는 코드입니다.
<br><br>

## 2. 사용 예시

```swift 
import UIKit

class Utils {

    /**
     # version
     - Note: 현재 번들 버전 반환
     */
    static func version() -> String {
        return Bundle.main.infoDictionary!["CFBundleShortVersionString"] as! String
    }
}
```
공통적으로 사용하는 변수 및 함수들을 Utils 라는 공통 클래스를 생성하여 관리하는 예시입니다. <br>

```swift
Utils.version()
```
필요한 곳 어디서든 생성한 함수를 호출하여 사용합니다.<br><br>
