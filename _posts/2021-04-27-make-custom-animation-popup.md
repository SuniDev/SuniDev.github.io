---
title: "[iOS/Swift] Custom Animation Popup 만들기"
header:
  teaser: ""
categories:
  - iOS
tags:
  - iOS
  - Swift
toc: true
---

## 이번 글은 
UIView.animate를 사용하여, Custom Animation Popup을 만드는 방법입니다.<br>
해당 포스팅은 Storyboard intreface 기반 Swift 프로젝트입니다.
<br>

<video width="300" controls>
  <source src="{{ site.url }}{{ site.baseurl }}/assets/images/210427/make-custom-animation-popup-sample1.mp4" type="video/mp4">
</video>
<br>
<video width="300" controls>
  <source src="{{ site.url }}{{ site.baseurl }}/assets/images/210427/make-custom-animation-popup-sample2.mp4" type="video/mp4">
</video>
<br><br>


## 1. UIWindow+Ext.swift 준비

팝업 호출 뷰의 디폴트 값을 최상위 뷰로 하기 위해, 최상위 뷰 컨트롤러를 얻는 UIWindow Extension을 먼저 만들어 줍니다. <br>
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

## 2. SnapKit 코코아팟 설치

스토리보드가 아닌 소스 코드로 Auto Layout을 컨트롤 할 때, 쉽게 하기 위해 SnapKit을 설치하여 사용합니다.<br>

SnapKit은 iOS 및 OS X에서 Auto Layout을 쉽게 하기 위한 DSL(Domain-specific Language)입니다.
{: .notice--primary}

Podfile을 생성하여 SnapKit 을 추가하고 설치합니다.

~~~
# Uncomment the next line to define a global platform for your project
# platform :ios, '9.0'

target 'SNPopup' do
  # Comment the next line if you don't want to use dynamic frameworks
  use_frameworks!

  # Pods for SNPopup
  ######################## UI
  # AutoLayout 관련
  pod 'SnapKit'

end
~~~
참고 포스팅 : [[iOS/Xcode] CocoaPods(코코아팟) 사용하기](https://sunidev.github.io/ios/start-cocoapods/)
<br><br>

## 2. BasePopVC.swift 생성

먼저, UIViewController를 상속받는 BasePopVC를 생성합니다.<br>
BasePopVC.swift는 Custom Animation Popup 기능을 만들어, 필요한 팝업 클래스에 상속할 클래스입니다.

~~~swift
class BasePopVC: UIViewController {
    
    override func viewDidLoad() {
        super.viewDidLoad()
        
        self.view.layoutIfNeeded()
    }
}
~~~
<br><br>

## 3. 애니메이션 관련 enum 생성

BasePopVC.swift에 애니메이션에 필요한 enum을 선언합니다.

~~~swift
/**
 # (E) PopupPosition
 - Author: suni
 - Date:
 - Note: PopupVC에 애니메이션 시작 포지션을 정하는 enum
 */
enum PopupPosition: String {
    case top = "Top"
    case bottom = "Bottom"
    case left = "Left"
    case right = "Rigth"
    case center = "Center"
    case none = ""
}

/**
 # (E) PopupType
 - Author: suni
 - Date:
 - Note: PopupVC에 애니메이션 타입을 모아둔 enum
 */
enum PopupType: String {
    case fadeInOut = "Fade In Out"
    case move = "Move"
    case none = ""
}
~~~
<br><br>

## 4. 상수 선언

애니메이션 시간도 선언합니다.
~~~swift
public let ANIMATION_DURATION: TimeInterval = 0.5
~~~
<br><br>

## 5. BasePopVC.swift 

Custom Animation Popup 기능을 수행하는 BasePopVC 클래스 소스입니다.

~~~swift
class BasePopVC: UIViewController {
    
    // popup dim 투명도
    private final let DIM_ALPHA: CGFloat = 0.3
    
    // popup dim view
    @IBOutlet weak var vDim: UIView!
    // popup content view
    @IBOutlet weak var vPopup: UIView!
    
    // popup animation type
    private var type: PopupType = .none
    // popup push start position
    private var position: PopupPosition = .none
    
    override func viewDidLoad() {
        super.viewDidLoad()
        
        self.view.layoutIfNeeded()
    }
}
extension BasePopVC {
    
    /**
     # showAnim
     - Author: suni
     - Date:
     - Parameters:
         - vc : 팝업을 노출할 뷰컨트롤러
         - type : 팝업의 애니메이션 타입
         - position : 팝업 애니메이션 시작 포지션
         - parentAddView : 해당 뷰컨트롤러의 뷰를 적용할 부모 뷰컨트롤러의 뷰
         - completeion : 해당 화면 노출 애니메이션이 완료된 이후에 부모 뷰컨트롤러에서 처리할 클로저
     - Returns:
     - Note: 팝업 화면을 애니메이션을 넣어서 보이는 함수
     */
    func showAnim(vc: UIViewController? = UIApplication.shared.keyWindow?.visibleViewController, type: PopupType = .fadeInOut, position: PopupPosition = .none, parentAddView: UIView?, _ completion: @escaping ()->()) {
        guard let currentVC = vc else {
            completion()
            return
        }
        
        var pView = parentAddView
        
        if pView == nil {
            pView = vc?.view
        }
        
        guard let parentView = pView else {
            completion()
            return
        }
        
        self.type = type
        self.position = position
        
        currentVC.addChild(self)
        self.view.translatesAutoresizingMaskIntoConstraints = false
        
        parentView.addSubview(self.view)
        self.view.snp.makeConstraints {
            $0.edges.equalToSuperview()
        }
        
        switch type {
        case .fadeInOut:
            self.vDim.alpha = 0.0
            self.vPopup.alpha = 0.0
            self.view.layoutIfNeeded()
            
            UIView.animate(withDuration: ANIMATION_DURATION/2) { [weak self] in
                if let _self = self {
                    _self.vDim.alpha = _self.DIM_ALPHA
                }
            } completion: { (complete) in
                UIView.animate(withDuration: ANIMATION_DURATION/2) { [weak self] in
                    if let _self = self {
                        _self.vPopup.alpha = 1.0
                    }
                } completion: { (complete) in
                    completion()
                }
            }
            
        case .move:
            self.vDim.alpha = 0.0
            let originalTransform = self.vPopup.transform
            
            var moveX: CGFloat = 0.0
            var moveY: CGFloat = 0.0
            
            switch position {
            case .top:
                moveX = 0.0
                moveY = -(self.vPopup.frame.maxY)
            case .bottom:
                moveX = 0.0
                moveY = UIScreen.main.bounds.size.height - self.vPopup.frame.minY
            case .left:
                moveX = -(self.vPopup.frame.maxX)
                moveY = 0.0
            case .right:
                moveX = UIScreen.main.bounds.size.width - self.vPopup.frame.minX
                moveY = 0.0
            case .center:
                moveX = 0.0
                moveY = 0.0
            default:
                break
            }
            
            let hideTransform = originalTransform.translatedBy(x: moveX, y: moveY)
            self.vPopup.transform = hideTransform
            self.vPopup.alpha = 0.0
            
            UIView.animate(withDuration: ANIMATION_DURATION) { [weak self] in
                if let _self = self {
                    _self.vPopup.transform = originalTransform
                    _self.vPopup.alpha = 1.0
                    _self.vDim.alpha = _self.DIM_ALPHA
                }
            } completion: { (complete) in
                completion()
            }
        default:
            completion()
        }
    }
    
    /**
     # hideAnim
     - Author: suni
     - Date: 20.08.19
     - Parameters:
         - type : 팝업의 애니메이션 타입
         - position : 팝업 애니메이션 숨김 포지션
         - completeion : 해당 화면 숨김 애니메이션이 완료된 이후에 부모 뷰컨트롤러에서 처리할 클로저
     - Returns:
     - Note: 팝업 화면을 애니메이션을 넣어서 숨기는 함수
     */
    func hideAnim(type: PopupType = .none, position: PopupPosition = .none, _ completion: @escaping ()->()) {
        DispatchQueue.main.async {
            
            switch self.type {
            case .fadeInOut:
                UIView.animate(withDuration: ANIMATION_DURATION/2, animations: { [weak self] in
                    if let _self = self {
                        _self.vPopup.alpha = 0.0
                    }
                }) { (complete) in
                    UIView.animate(withDuration: ANIMATION_DURATION/2, animations: { [weak self] in
                        self?.vDim.alpha = 0.0
                    }) { [weak self] complete in
                        if let _self = self {
                            _self.view.removeFromSuperview()
                            _self.removeFromParent()
                        }
                    }
                }
                break
            case .move:
                let originalTransform = self.vPopup.transform
                
                var moveX: CGFloat = 0.0
                var moveY: CGFloat = 0.0
                
                switch position {
                case .top:
                    moveX = 0.0
                    moveY = -(self.vPopup.frame.maxY)
                case .bottom:
                    moveX = 0.0
                    moveY = UIScreen.main.bounds.size.height - self.vPopup.frame.minY
                case .left:
                    moveX = -(self.vPopup.frame.maxX)
                    moveY = 0.0
                case .right:
                    moveX = UIScreen.main.bounds.size.width - self.vPopup.frame.minX
                    moveY = 0.0
                case .center:
                    moveX = 0.0
                    moveY = 0.0
                default:
                    break
                }
                
                let hideTransform = originalTransform.translatedBy(x: moveX, y: moveY)
                                
                UIView.animate(withDuration: ANIMATION_DURATION, animations: { [weak self] in
                    if let _self = self {
                        _self.vDim.alpha = 0.0
                        _self.vPopup.alpha = 0.0
                        _self.vPopup.transform = hideTransform
                    }
                }) { [weak self] complete in
                    if let _self = self {
                        _self.view.removeFromSuperview()
                        _self.removeFromParent()
                        completion()
                    }
                }
                break
            default:
                completion()
            }
        }
    }
    
    @IBAction func btnCancelPressed(_ sender: UIButton) {
        self.hideAnim(type: self.type, position: self.position) {
            
        }
    }
    
    @IBAction func btnCompletePressed(_ sender: UIButton) {
        self.hideAnim(type: self.type, position: self.position) {
            
        }
    }
}
~~~

## 6. 사용 예제

BasePopVC를 상속받는 커스텀 팝업 클래스를 생성합니다.
 
~~~swift
import UIKit

class SNPopVC: BasePopVC {

    override func viewDidLoad() {
        super.viewDidLoad()

    }

}
~~~

Storyboard에서 팝업을 커스텀하여 만든 뒤,<br>
![image]({{ site.url }}{{ site.baseurl }}/assets/images/icon/icon4.png) > Custom Class > Class 에 팝업의 클래스명을 입력합니다.<br>
Identity > StoryboardID 에 클래스명을 입력한 뒤, Use Storyboard ID를 입력합니다.<br>
![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/210427/make-custom-animation-popup1.png){: .align-center}

<br>
BasePopVC에 vDim과 vPopup을 연결합니다.<br>
![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/210427/make-custom-animation-popup2.png){: .align-center}
![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/210427/make-custom-animation-popup3.png){: .align-center}

<br>
BasePopVC에 btnCompleteProssed Action을 연결합니다.<br>
![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/210427/make-custom-animation-popup4.png){: .align-center}

<br>
이제 아래 코드로 팝업을 호출하면 됩니다.<br>

~~~swift
let storyBoard: UIStoryboard = UIStoryboard(name: "Main", bundle: nil)
let popVC = storyBoard.instantiateViewController(withIdentifier: "SNPopVC") as! SNPopVC

popVC.showAnim(vc: self, type: .move, position: .bottom, parentAddView: self.view) { }
~~~

<br><br>

## 프로젝트 소스 GitHub
[SNPopup 다운받으러 가기](https://github.com/SuniDev/SNPopup)
