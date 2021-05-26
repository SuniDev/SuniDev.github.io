---
title: "[iOS/Xcode] CocoaPods(코코아팟) 사용하기"
header:
  teaser: ""
categories:
  - iOS
tags:
  - iOS
  - Xcode
  - CocoaPods
toc: true
---


"CocoaPods는 Swift 및 Objective-C 코코아 프로젝트의 종속성 관리자입니다. 28,000개가 넘는 라이브러리를 가지고 있으며 170만개가 넘는 응용 프로그램(앱)에서 사용되고 있습니다. CocoaPod은 프로젝트를 우아하게 확장할 수 있도록 도와줍니다." - [CocoaPods 사이트](https://cocoapods.org/)
<br><br>

## 이번 글은 
CocoaPods 를 사용하는 방법입니다. 
<br><br>


## 1. 코코아팟 설치하기

터미널을 열고 아래의 명령어를 입력해 줍니다.
~~~bash
$ sudo gem install cocoapods
~~~
그럼 코코아팟을 사용할 준비가 끝났습니다!


## 2. 프로젝트에서 코코아팟 사용하기 1

터미널에서 Xcode 프로젝트 위치로 이동합니다.
~~~bash
$ cd {Xcode 프로젝트 위치}
~~~ 
<br>

Podfile 을 생성합니다.
~~~bash
$ pod init
~~~
<br>

프로젝트 폴더에 생성된 Podfile 을 열고, 사용할 pods 를 넣습니다.
~~~
# Uncomment the next line to define a global platform for your project
# platform :ios, '9.0'

target 'SampleProject' do
  # Comment the next line if you don't want to use dynamic frameworks
  use_frameworks!

  # Pods for SampleProject
  pod 'RxSwift'
  pod 'RxCocoa'

end
~~~
<br>

Podfile 이 완성되면, 저장 후 다시 터미널로 돌아가 pods 을 설치합니다.
~~~bash
$ pod install
~~~
![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/210426/start-cocoapods1.png)
<br><br>

## 3. 프로젝트에서 코코아팟 사용하기 2

이제 프로젝트 디렉토리를 열어보면,<br>
세 가지 파일과 디렉토리가 생성되었습니다.<br>
![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/210426/start-cocoapods2.png)

    * Podfile.lock : Pods의 버전픽스를 위한 파일
    * Pods : 라이브러리들이 다운로드 되는 디렉토리
    * {프로젝트명}.wcworkspace: Pods를 사용할 수 있도록 포함된 워크스페이스.<br>
      -> 이제는 wcworkspace로 프로젝트를 열어 작업해야 합니다.
{: .notice--primary}


wcworkspace 를 실행해 보면,<br>
왼쪽 프로젝트 네비게이터에 Pods 프로젝트와 Pods 가 설치된 것을 볼 수 있습니다.<br>
![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/210426/start-cocoapods3.png)
<br>

이제 프로젝트에서 설치된 Pods를 import 하여 사용할 수 있습니다.<br>
```swift
import UIKit
import RxSwift
import RxCocoa

class ViewController: UIViewController {

    override func viewDidLoad() {
        super.viewDidLoad()
        // Do any additional setup after loading the view.
    }
}
```

<br><br>
**참고**<br>
[iOS 프로젝트에 cocoapods 적용하기](http://monibu1548.github.io/2018/04/16/cocoapods/)
