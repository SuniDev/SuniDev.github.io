---
title: "[iOS/Swift] 디바이스 고유넘버(device uuid) 구하기"
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

iOS4까지는 iOS기기의 고유 넘버로 udid를 사용하였으나, 개인정보 문제로 iOS5부터는 udid가 사라지고 uudi(임의로 생성한 고유값)를 사용합니다. <br><br>

## 이번 글은 
디바이스 고유넘버(device uuid)를 구하는 방법입니다.<br><br>

## 1. uuidString

```swift 
func getDeviceUUID() -> String {
    return UIDevice.current.identifierForVendor!.uuidString
}
```
디바이스의 고유 넘버를 구하는 함수입니다.
<br><br>

## 2. 사용 예시

```swift 
import UIKit

class Utils {

    /**
     # getDeviceUUID
     - Note: 디바이스 고유 넘버 반환
     */
    static func getDeviceUUID() -> String {
        return UIDevice.current.identifierForVendor!.uuidString
    }
}
```
공통적으로 사용하는 변수 및 함수들을 Utils 라는 공통 클래스를 생성하여 관리하는 예시입니다. <br>

```swift
Utils.getDeviceUUID()
```
필요한 곳 어디서든 생성한 함수를 호출하여 사용합니다.<br><br>


## 3. notice

uuid는 앱을 삭제하면 새롭게 생성 됩니다. <br>
앱을 재설치해도 고유한 번호가 필요한 경우, 최초 uuid 생성 시점에 keychain에 저장하는 방법을 사용하세요.
{: .notice--primary}
