---
title: "[iOS/Xcode] http로 시작하는 URL 허용하기"
header:
  teaser: ""
categories:
  - iOS
  - ISSUENOTE
tags:
  - iOS
  - Xcode
toc: true
---



iOS9부터 HTTP 접근을 허용하지 않습니다. <br>


## 이번 글은 
iOS9이상 버전부터 HTTP URL을 허용하는 방법입니다.<br><br>

## 1. info.plist
```
<key>NSAppTransportSecurity</key>
<dict>
    <key>NSAllowsArbitraryLoads</key>
    <true/>
</dict>
```
info.plist에 해당 Source Code를 추가하거나
<br><br>


![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/210629/allow-http-url-plist.png){: .align-center}
사진처럼 App Transport Security Settings > Allow Arbitrary Loads를 추가합니다.
<br><br>
