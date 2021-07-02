---
title: "[iOS/Swift] 특정 상황에 VoiceOver 알림 등록하기"
header:
  teaser: ""
categories:
  - iOS
  - ISSUENOTE
tags:
  - iOS
  - Swift
  - UIAccessibility
toc: true
---


특정 상황에 Notification 처럼 VoiceOver가 알리는 기능이 필요한 경우 참고하세요.<br>


## 이번 글은 
UIAccessibility.post(notification:argument:) 을 사용하여<br>
특정 상황에 Voice Over가 알리는 기능을 구현하는 방법입니다.<br><br>

## 1. UIAccessibility.post(notification:argument:)

[Apple Developer](https://developer.apple.com/documentation/uikit/uiaccessibility/1615194-post) 를 확인하면,
앱에 접근성 알림을 게시해야 할 때, 사용하라고 써있습니다.

## 2. Source Code

```swift
UIAccessibility.post(notification: .announcement, argument: "관심 등록 완료")
```
알림이 필요한 부분에 위 소스코드를 추가하면 됩니다.<br><br>

