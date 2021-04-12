---
title: "[iOS/Swift] UIAlertController를 사용하여 Alert 만들기"
header:
  teaser: ""
categories:
  - iOS
tags:
  - iOS
  - Swift
  - UIAlertController
toc: true
---

## 이번 글은 
![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/210412/make-common-alert-alert3.PNG){: .align-center}
<br><br>
![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/210412/make-common-alert-actionsheet3.PNG){: .align-center}
UIAlertController를 사용하여 어디서든 얼럿을 노출할 수 있는 static 클래스 소스입니다.
<br><br>

## 1. UIWindow+Ext.swift 준비

얼럿의 경우 최상위 뷰에 띄우는 경우가 대부분입니다. <br>
얼럿 호출 뷰의 디폴트 값을 최상위 뷰로 하기 위해, 최상위 뷰 컨트롤러를 얻는 UIWindow Extension을 먼저 만들어 줍니다. <br>
참고 블로그 포스팅 : [[iOS/Swift] 최상위에 있는 뷰컨트롤러 얻기](https://sunidev.github.io/ios/visibleviewcontroller/)

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
<br><br>

## 2. CommonAlert.swift

```swift
    import UIKit

    class CommonAlert {
        
        /**
         # showAlertAction1
         - Author: suni
         - Date:
         - Parameters:
            - vc: 알럿을 띄울 뷰컨트롤러
            - preferredStyle: 알럿 스타일
            - title: 알럿 타이틀명
            - message: 알럿 메시지
            - completeTitle: 확인 버튼명
            - completeHandler: 확인 버튼 클릭 시, 실행될 클로저
         - Returns:
         - Note: 버튼이 1개인 알럿을 띄우는 함수
        */
        static func showAlertAction1(vc: UIViewController? = UIApplication.shared.keyWindow?.visibleViewController, preferredStyle: UIAlertController.Style = .alert, title: String = "", message: String = "", completeTitle: String = "확인", _ completeHandler:(() -> Void)? = nil){
            
            guard let currentVc = vc else {
                completeHandler?()
                return
            }
            
            DispatchQueue.main.async {
                let alert = UIAlertController(title: title, message: message, preferredStyle: preferredStyle)
                
                let completeAction = UIAlertAction(title: completeTitle, style: .default) { action in
                    completeHandler?()
                }
                
                alert.addAction(completeAction)
                
                currentVc.present(alert, animated: true, completion: nil)
            }
        }
        
        /**
         # showAlertAction2
         - Author: suni
         - Date:
         - Parameters:
            - vc: 알럿을 띄울 뷰컨트롤러
            - preferredStyle: 알럿 스타일
            - title: 알럿 타이틀명
            - message: 알럿 메시지
            - cancelTitle: 취소 버튼명
            - completeTitle: 확인 버튼명
            - cancelHandler: 취소 버튼 클릭 시, 실행될 클로저
            - completeHandler: 확인 버튼 클릭 시, 실행될 클로저
         - Returns:
         - Note: 버튼이 2개인 알럿을 띄우는 함수
        */
        static func showAlertAction2(vc: UIViewController? = UIApplication.shared.keyWindow?.visibleViewController, preferredStyle: UIAlertController.Style = .alert, title: String = "", message: String = "", cancelTitle: String = "취소", completeTitle: String = "확인",  _ cancelHandler: (() -> Void)? = nil, _ completeHandler: (() -> Void)? = nil){
            
            guard let currentVc = vc else {
                completeHandler?()
                return
            }
            
            DispatchQueue.main.async {
                let alert = UIAlertController(title: title, message: message, preferredStyle: preferredStyle)
                
                let cancelAction = UIAlertAction(title: cancelTitle, style: .cancel) { action in
                    cancelHandler?()
                }
                let completeAction = UIAlertAction(title: completeTitle, style: .default) { action in
                    completeHandler?()
                }
                
                alert.addAction(cancelAction)
                alert.addAction(completeAction)
                
                currentVc.present(alert, animated: true, completion: nil)
            }
        }
        
        /**
         # showAlertAction3
         - Author: suni
         - Date:
         - Parameters:
            - vc: 알럿을 띄울 뷰컨트롤러
            - preferredStyle: 알럿 스타일
            - title: 알럿 타이틀명
            - message: 알럿 메시지
            - cancelTitle: 취소 버튼명
            - completeTitle: 확인 버튼명
            - destructiveTitle: 삭제 버튼명
            - cancelHandler: 취소 버튼 클릭 시, 실행될 클로저
            - completeHandler: 확인 버튼 클릭 시, 실행될 클로저
            - destructiveHandler: 삭제 버튼 클릭 시, 실행될 클로저
         - Returns:
         - Note: 버튼이 3개인 알럿을 띄우는 함수
        */
        static func showAlertAction3(vc: UIViewController? = UIApplication.shared.keyWindow?.visibleViewController, preferredStyle: UIAlertController.Style = .alert, title: String = "", message: String = "", cancelTitle: String = "취소", completeTitle: String = "확인", destructiveTitle: String = "삭제", _ cancelHandler:(() -> Void)? = nil, _ completeHandler:(() -> Void)? = nil, _ destructiveHandler:(() -> Void)? = nil){
            
            guard let currentVc = vc else {
                completeHandler?()
                return
            }
            
            DispatchQueue.main.async {
                let alert = UIAlertController(title: title, message: message, preferredStyle: preferredStyle)
                
                let cancelAction = UIAlertAction(title: cancelTitle, style: .cancel) { action in
                    cancelHandler?()
                }
                let destructiveAction = UIAlertAction(title: destructiveTitle, style: .destructive) { action in
                    cancelHandler?()
                }
                let completeAction = UIAlertAction(title: completeTitle, style: .default) { action in
                    completeHandler?()
                }
                
                alert.addAction(cancelAction)
                alert.addAction(destructiveAction)
                alert.addAction(completeAction)
                
                currentVc.present(alert, animated: true, completion: nil)
            }
        }
    }

```
어디서든 여러 타입의 얼럿을 쉽게 가져와 쓰기 위해, 얼럿 노출 기능을 static 함수로 만든 소스입니다.
<br><br>

## 3. 사용 예제 (preferredStyle = .alert)

UIAlertController.Style이 .alert인 사용 예제 소스입니다.<br><br>

버튼 1개<br>
![image]({{ site.url }}{{ site.baseurl }}/assets/images/210412/make-common-alert-alert1.PNG)
<br>
```swift
    CommonAlert.showAlertAction1(vc: self, preferredStyle: .alert, title: "Alert Style", message: "1 Button Alert")
```
<br>
버튼 2개<br>
![image]({{ site.url }}{{ site.baseurl }}/assets/images/210412/make-common-alert-alert2.PNG)
<br>
```swift
    CommonAlert.showAlertAction2(vc: self, preferredStyle: .alert, title: "Alert Style", message: "2 Button Alert")
```
<br>
버튼 3개<br>
![image]({{ site.url }}{{ site.baseurl }}/assets/images/210412/make-common-alert-alert3.PNG)
<br>
```swift
    CommonAlert.showAlertAction3(vc: self, preferredStyle: .alert, title: "Alert Style", message: "3 Button Alert")
```
<br>

## 4. 사용 예제 (preferredStyle = .actionSheet)

UIAlertController.Style이 .actionSheet인 사용 예제 소스입니다.<br><br>

버튼 1개<br>
![image]({{ site.url }}{{ site.baseurl }}/assets/images/210412/make-common-alert-actionsheet1.PNG)
<br>
```swift
    CommonAlert.showAlertAction1(vc: self, preferredStyle: .actionSheet, title: "Action Sheet Style", message: "1 Button Action Sheet")
```
<br>
버튼 2개<br>
![image]({{ site.url }}{{ site.baseurl }}/assets/images/210412/make-common-alert-actionsheet2.PNG)
<br>
```swift
    CommonAlert.showAlertAction2(vc: self, preferredStyle: .actionSheet, title: "Action Sheet Style", message: "2 Button Action Sheet")
```
<br>
버튼 3개<br>
![image]({{ site.url }}{{ site.baseurl }}/assets/images/210412/make-common-alert-actionsheet3.PNG)
<br>
```swift
    CommonAlert.showAlertAction3(vc: self, preferredStyle: .actionSheet, title: "Action Sheet Style", message: "3 Button Action Sheet")
```
<br><br>

## 프로젝트 소스 GitHub
[SNAlert 다운받으러 가기](https://github.com/SuniDev/SNAlert)
