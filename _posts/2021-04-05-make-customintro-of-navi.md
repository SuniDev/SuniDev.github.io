---
title: "[iOS/Swift] UINavigationController를 사용하여 Custom Intro 만들기"
header:
  teaser: ""
categories:
  - iOS
tags:
  - iOS
  - Swift
  - Intro
  - UINavigationController
toc: true
---


요즘 자주 사용하게 될 구조를 미리 만들어놓고 실무에 곧바로 가져다 쓰기 위한 작업 중이에요. ^_^<br>
블로그 포스팅을 위해 분할 작업을 먼저 하고 취합해서 github에 올려놓을 예정입니다.  <br>
실무를 하면서 항상 개발하는 기능 중 하나가 인트로 화면에서 데이터를 가져온 뒤 메인으로 가는 작업이에요.<br>
이에 대한 UINavigationController 기반 구조 개발 포스팅입니다.<br><br>


## 이번 글은 
<video width="300" controls>
  <source src="{{ site.url }}{{ site.baseurl }}/assets/images/210405/make-customintro-of-navi-sample.mp4" type="video/mp4">
</video>
UINavigationController를 사용하여 Custom Intro를 만드는 방법입니다.
해당 포스팅은 Storyboard intreface 기반 Swift 프로젝트입니다.
<br><br>


## 1. LaunchScreen.storyboard

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/210405/make-customintro-of-navi-launch.png){: .align-center}
먼저 LaunchScreen 을 작업합니다.
저는 인트로 화면과 런치 스크린을 똑같이 그렸습니다.
<br><br>


## 2. 인트로 화면 생성 및 설정 1 - Intro.storyboard

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/210405/make-customintro-of-navi-introsb1.png){: .align-center}
Intro 화면을 작업할 스토리보드를 생성합니다. 
<br>

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/210405/make-customintro-of-navi-introsb2.png){: .align-center}
Intro.storyboard에 ViewController를 추가하여<br>
인트로 화면을 원하는 대로 커스텀 합니다.
<br><br>


## 3. 인트로 화면 생성 및 설정 2 - IntroVC.swift

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/210405/make-customintro-of-navi-introvc1.png){: .align-center}
![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/210405/make-customintro-of-navi-introvc2.png){: .align-center}
인트로 화면 기능을 수행해 줄 UIViewController를 상속받는 IntroVC 클래스를 생성합니다.
<br>

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/210405/make-customintro-of-navi-introvc3.png){: .align-center}
Intro.storybaord > ViewController 선택 > ![image]({{ site.url }}{{ site.baseurl }}/assets/images/210405/icon4-white.png) > Custom Class > IntroVC 입력 <br>
Identity > Storyboard ID > IntroVC 입력<br>
Use Storyboard ID를 체크합니다.
<br><br>


## 4. AppDelegate.swift


```swift
var navigationVC: UINavigationController?
```
AppDelegate.swift > UINavigationController를 선언합니다.
<br>

```swift
extension AppDelegate {
    /**
     # initNavigationVC
     - Author: suni
     - Date:
     - Note: Root Navigation 초기화
    */
    func initNavigationVC() {
        self.navigationVC = UINavigationController()
        self.navigationVC?.isNavigationBarHidden = true
        
        let storyBoard: UIStoryboard = UIStoryboard(name: "Intro", bundle: nil)
        let introVC = storyBoard.instantiateViewController(withIdentifier: "IntroVC") as! IntroVC
        self.navigationVC?.setViewControllers([introVC], animated: false)
        
        self.window = UIWindow(frame: UIScreen.main.bounds)
        self.window!.rootViewController = navigationVC
        self.window!.makeKeyAndVisible()
    }
}
```
AppDelegate extension 을 선언하여, 루트 내비게이션을 초기화하는 함수를 생성합니다.
<br>

```swift 
func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplication.LaunchOptionsKey: Any]?) -> Bool {

    // Root Navigation 초기화
    self.initNavigationVC()

    return true
}
```
AppDelegate.swift > didFinishLaunchingWithOptions에 생성한 내비게이션 초기화 함수를 호출합니다.
<br><br>


## 5. 메인 화면 생성 및 설정 1 - Main.storyboard


![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/210405/make-customintro-of-navi-mainsb.png){: .align-center}
Main.storyboard에 메인 화면을 원하는 대로 커스텀 합니다.
<br><br>


## 6. 메인 화면 생성 및 설정 2 - MainVC.swift


![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/210405/make-customintro-of-navi-mainvc1.png){: .align-center}
![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/210405/make-customintro-of-navi-mainvc2.png){: .align-center}
메인 화면 기능을 수행해 줄 UIViewController를 상속받는 MainVC 클래스를 생성합니다.

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/210405/make-customintro-of-navi-mainvc3.png){: .align-center}
Main.storybaord > ViewController 선택 > ![image]({{ site.url }}{{ site.baseurl }}/assets/images/210405/icon4-white.png) > Custom Class > MainVC 입력 <br>
Identity > Storyboard ID > MainVC 입력<br>
Use Storyboard ID를 체크합니다.
<br><br>


## 7. IntroVC.swift


```swift
extension IntroVC {
    /**
     # moveMain
     - Author: suni
     - Date:
     - Note: 메인 화면 이동
    */
    func moveMain() {
        let sb = UIStoryboard(name: "Main", bundle: nil)
        let vc = sb.instantiateViewController(withIdentifier: "MainVC") as! MainVC
        
        if let appDelegate = UIApplication.shared.delegate as? AppDelegate {
            let transition: CATransition = CATransition()
            transition.duration = 0.4
            transition.timingFunction = CAMediaTimingFunction(name: .easeInEaseOut)
            transition.type = .fade
            appDelegate.navigationVC?.view.layer.add(transition, forKey: nil)
            
            appDelegate.navigationVC?.setViewControllers([vc], animated: false)
        }
        
    }
}
```
IntroVC extension 을 선언하여, AppDelegate의 naviagtionVC를 이용한 메인 화면(MainVC)로 이동하는 함수를 생성합니다.
<br>

```swift
override func viewDidLoad() {
    super.viewDidLoad()
    // Do any additional setup after loading the view.
    
    let time = DispatchTime.now() + 2.0
    DispatchQueue.main.asyncAfter(deadline: time) {
        self.moveMain()
    }
}
```
IntroVC.swift > viewDidLoad() 내에서 메인 화면 이동 함수를 호출합니다.
<br>

위 코드는 2.0초 뒤에 인트로 화면 > 메인 화면으로 전환하는 코드입니다.<br>
만약 인트로에서 메인 화면 데이터 불러오기, 버전 체크 등 필수로 처리해야 할 일이 있으면 완료 후, 메인 화면으로 이동하는 로직도 있습니다.
{: .notice--primary}

<br><br>


## 프로젝트 소스 GitHub
[SNIntro 다운받으러 가기](https://github.com/SuniDev/SNIntro)
