---
title: "[iOS/Swift] 시뮬레이터(Simulator) 구동 여부 확인하기"
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
현재 시뮬레이터 구동 여부를 확인하는 방법입니다.<br><br>

## 1. SIMULATOR_DEVICE_NAME

```swift 
func isSimulator() -> Bool {
    return ProcessInfo.processInfo.environment["SIMULATOR_DEVICE_NAME"] != nil
}
```
시뮬레이터가 구동 중이면 true를 반환하는 함수입니다.
<br><br>

## 2. 사용 예시

```swift 
import UIKit

class Utils {

    /**
     # isSimulator
     - Returns: Bool
     - Note: 시뮬레이터 구동 여부를 반환하는 함수.
     */
    static func isSimulator() -> Bool {
        return ProcessInfo.processInfo.environment["SIMULATOR_DEVICE_NAME"] != nil
    }
}
```
공통적으로 사용하는 변수 및 함수들을 Utils 라는 공통 클래스를 생성하여 관리하는 예시입니다. <br>

```swift
Utils.isSimulator()
```
필요한 곳 어디서든 생성한 함수를 호출하여 사용합니다.<br><br>
