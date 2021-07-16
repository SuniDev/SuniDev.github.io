---
title: "[iOS/Swift] 시간을 다른 형태로 변형하기 (DateFormatter)"
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
시간을 다른 형태로 변형하는 방법입니다.<br><br>

## 1. DateFormatter()

```swift 
extension Date {

    /**
     # formatted
     - Parameters:
        - format: 변형할 DateFormat
     - Note: DateFormat으로 변형한 String 반환
    */
    public func formatted(_ format: String) -> String {
        let formatter = DateFormatter()
        formatter.dateFormat = format
        formatter.timeZone = TimeZone(identifier: TimeZone.current.identifier)!
        
        return formatter.string(from: self)
    }
}
```
Date extension에 Date를 특정 형태의 String값으로 변형하여 반환하는 함수를 생성합니다.<br>

TimeZone.current.identifier은 디바이스 기준 TimeZone 값 입니다.
{: .notice--primary}

<br><br>

## 2. 사용 예시

```swift
// 현재 시간 표시
Date().formatted("yyyy-MM-dd HH:mm:ss")

// 결과 : 2021-07-16 15:18:23
```
