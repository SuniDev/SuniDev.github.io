---
title: "[iOS/Swift] 현재 시간의 밀리초 구하기"
header:
  teaser: ""
categories:
  - iOS
  - ISSUENOTE
tags:
  - iOS
  - Swift
  - Extension
toc: true
---

## 이번 글은 
현재 시간의 밀리초를 구하는 방법입니다.<br><br>

## 1. timeIntervalSince1970

``` swift
Int(Date().timeIntervalSince1970 / 1000.0)
```

현재 시간의 밀리초를 구하는 코드입니다.
<br><br>

## 2. 사용 예시
```swift 

import Foundation

extension Date {
    /**
     # currentTimeInMilli
     - Note: 현재 시간의 밀리초 반환
    */
    public static func currentTimeInMilli() -> Int {
        return Date().timeInMilli()
    }

    /**
     # timeInMilli
     - Note: timeIntervalSince1970의 밀리초 반환
    */
    public func timeInMilli() -> Int {
        return Int(self.timeIntervalSince1970 / 1000.0)
    }
}
```
extension에 자주 사용하는 기능을 함수로 생성해 관리하는 예시입니다.<br>
Date extension에 밀리초를 반환하는 함수를 생성합니다.<br>

```swift
// 현재 시간의 밀리초
Date().currentTimeInMilli()
```
필요한 곳에서 생성한 함수를 호출하여 사용합니다.

<br><br>
