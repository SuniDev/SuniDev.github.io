---
title: "[iOS/Swift] 현재 디바이스 SafeArea의 top/bottom 영역 값 구하기 "
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

iPhoneX 부터는 노치 영역으로 인해 레이아웃이 깨져 골치 아플 때가 많습니다.<br>
그래서 SafeArea Inset값이 디바이스마다 변동되어 자주 필요로 합니다.<br><br>


## 이번 글은 
현재 디바이스의 SafeArea의 top.bottom 영역 값을 구하는 방법입니다.<br><br>

## 1. SafeArea Top inset

```swift 
func safeAreaTopInset() -> CGFloat {
    let statusHeight = UIApplication.shared.statusBarFrame.size.height   // 상태바 높이
    
    if #available(iOS 11.0, *) {
        let window = UIApplication.shared.keyWindow
        let topPadding = window?.safeAreaInsets.top
        return topPadding ?? statusHeight
    } else {
        return statusHeight
    }
}
```
Safe Area Top 영역 값을 리턴하는 함수입니다.<br>

노치가 없는 디바이스는 상태바 높이가 top 영역 값입니다.
{: .notice--primary}

<br><br>

## 2. SafeArea Bottom inset

```swift 
func safeAreaBottomInset() -> CGFloat {
    if #available(iOS 11.0, *) {
        let window = UIApplication.shared.keyWindow
        let bottomPadding = window?.safeAreaInsets.bottom
        return bottomPadding ??  0.0
    } else {
        return 0.0
    }
}
```
Safe Area Bottom 영역 값을 리턴하는 함수입니다. 
<br><br>

## 3. 사용 예시

```swift 
import UIKit

class Utils {
    public static let STATUS_HEIGHT = UIApplication.shared.statusBarFrame.size.height   // 상태바 높이
    
    /**
     # safeAreaTopInset
     - Note: 현재 디바이스의 safeAreaTopInset값을 리턴하는 함수
     */
    static func safeAreaTopInset() -> CGFloat {
        if #available(iOS 11.0, *) {
            let window = UIApplication.shared.keyWindow
            let topPadding = window?.safeAreaInsets.top
            return topPadding ?? Utils.STATUS_HEIGHT
        } else {
            return Utils.STATUS_HEIGHT
        }
    }
    
    /**
     # safeAreaBottomInset
     - Note: 현재 디바이스의 safeAreaBottomInset값을 리턴하는 함수
     */
    static func safeAreaBottomInset() -> CGFloat {
        if #available(iOS 11.0, *) {
            let window = UIApplication.shared.keyWindow
            let bottomPadding = window?.safeAreaInsets.bottom
            return bottomPadding ??  0.0
        } else {
            return 0.0
        }
    }
}
```
공통적으로 사용하는 변수 및 함수들을 Utils 라는 공통 클래스를 생성하여 관리하는 예시입니다. <br>

```swift
Utils.safeAreaTopInset()        
Utils.safeAreaBottomInset()
```
필요한 곳 어디서든 생성한 함수를 호출하여 사용합니다.<br><br>
