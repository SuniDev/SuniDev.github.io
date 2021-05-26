---
title: "[iOS/Swift] 최상위에 있는 뷰컨트롤러 얻기"
header:
  teaser: ""
categories:
  - iOS
tags:
  - iOS
  - Swift
  - UIWindow
  - Extension
toc: true
---


개발하면서 앱의 최상위 뷰 컨트롤러를 찾아야 할 일이 종종 있습니다.<br>
처음에는 최상위 뷰에 얼럿을 띄우기 위해 작업을 했지만, 한번 extension으로 빼놓으니 1) 웹뷰 통신(브릿지 호출)으로 뷰 이동 2) 푸시나 스키마를 통해 딥링크가 들어올 때, 가동 중인 앱의 뷰 이동 등 여러 곳에서 사용할 수 있게 되었습니다. <br><br>


## 이번 글은 
현재 앱에서 최상위 뷰 컨트롤러를 반환하는 UIWindow Extension 소스입니다.
<br><br>


## 1. UIWindow+Ext.swift

```swift

import UIKit

extension UIWindow {
    
    public var visibleViewController: UIViewController? {
        return self.visibleViewControllerFrom(vc: self.rootViewController)
    }
    
    /**
     # visibleViewControllerFrom
     - Author: suni
     - Date: 
     - Parameters:
        - vc: rootViewController 혹은 UITapViewController
     - Returns: UIViewController?
     - Note: vc내에서 가장 최상위에 있는 뷰컨트롤러 반환
    */
    public func visibleViewControllerFrom(vc: UIViewController? = UIApplication.shared.keyWindow?.rootViewController) -> UIViewController? {
        if let nc = vc as? UINavigationController {
            return self.visibleViewControllerFrom(vc: nc.visibleViewController)
        } else if let tc = vc as? UITabBarController {
            return self.visibleViewControllerFrom(vc: tc.selectedViewController)
        } else {
            if let pvc = vc?.presentedViewController {
                return self.visibleViewControllerFrom(vc: pvc)
            } else {
                return vc
            }
        }
    }
}

```


## 2. 사용 예제

```swift
    if let vc = UIApplication.shared.keyWindow?.visibleViewController as? UIViewController {
        let alert = UIAlertController(title: "타이틀", message: "메시지", preferredStyle:UIAlertController.Style.alert)
        let action1 = UIAlertAction(title:completeTitle, style: .default) { action in
            completeHandler?()
        }
        alert.addAction(action1)
        vc.present(alert, animated: true, completion: nil)
    }
    
```
최상위 뷰에 얼럿을 띄우는 예제입니다.

<br><br>
**참고**<br>
[[SWIFT] 현재 실행 중(혹은 실행할) 앱의 최상 뷰 컨트롤러 얻기](https://g-y-e-o-m.tistory.com/93)
