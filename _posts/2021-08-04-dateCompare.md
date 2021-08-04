---
title: "[iOS/Swift] 날짜 비교해서 과거/현재/미래 구하기"
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
두 날짜를 비교해서 과거/현재/미래를 구하는 방법입니다.<br><br>

## 1. DateFormatter()

```swift 
extension Date {

    /**
     # dateCompare
     - Parameters:
        - fromDate: 비교 대상 Date
     - Note: 두 날짜간 비교해서 과거(Future)/현재(Same)/미래(Past) 반환
    */
    public func dateCompare(fromDate: Date) -> String {
        var strDateMessage:String = ""
        let result:ComparisonResult = self.compare(fromDate)
        switch result {
        case .orderedAscending:
            strDateMessage = "Future"
            break
        case .orderedDescending:
            strDateMessage = "Past"
            break
        case .orderedSame:
            strDateMessage = "Same"
            break
        default:
            strDateMessage = "Error"
            break
        }
        return strDateMessage
    }
}
```
두 날짜간 비교해서 과거(Future)/현재(Same)/미래(Past) 반환하는 함수를 Date Extension에 생성합니다.

<br><br>

## 2. 사용 예시

```swift

// 현재 날짜와 date 날짜 비교
Date().dateCompare(fromDate: date)


// date1 날짜와 date2 날짜 비교
date1.dateCompare(fromDate: date2)

```
