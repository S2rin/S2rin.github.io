---
Layout: post
Title: iOS Study 6장 - UIViewController
Tags: ios study surin
---

> 화면이 여러개 있는 애플리케이션 만드는 방법에 대해서 알아보자
>
> 작성자: 김수린, 마지막 수정: 2017.01.01

**Index**

* TOC

**Task**

* 스위프트로 시작하는 아이폰 앱 개발 교과서 (위키북스) 6장

---

### 1.화면이 여러 개 있는 애플리케이션이란?

여러 개의 화면을 준비하고, 화면과 화면을 연결해서 만드는 애플리케이션

> **주의사항**
>
> * 화면 변경 방법 결정
> * 화면마다 뷰 컨트롤러(ViewController) 만들기
> * 데이터 연동 방법 결정

#### 1.1 화면 변경 방법 설정

어떤 화면에서 다른 화면으로 변경하는 것은 간단한 연결로 구현이 가능합니다. 이때 중요한 것은 애플리케이션의 특징에 따라 적합한 방법을 '선택'하는 것입니다.

* 화면 위에 띄워 변경: **Modal**

  화면 위에 화면을 띄우는 방법

  일시적인 화면에 사용

* 내비게이션으로 변경: **Navigation Controller**

  애플리케이션에 내비게이션 바가 있고, 계층적으로 화면을 전환하는 방법

  여러 개의 화면을 계층적으로 구성하고 싶을 때 사용

  Master-Detail Application 템플릿

* 탭바로 변경 : **Tab Bar Controller**

  애플리케이션 아래에 탭바가 있고, 병렬적으로 화면을 전환하는 방법

  탭 바에 있는 버튼 하나를 탭하면 탭바 위에 있는 화면이 전환

  Tabbed Application 템플릿

> 화면 변경 방법은 세그웨이로 지정

어떤 화면에서 다른 화면으로 변경할 때는 세그웨이(Segue)를 사용해서 연결합니다.

'세그웨이'는 Xcode에서 어떤 화면으로 이동할지 그리고 어떤 방식으로 화면을 변경할 지 지정합니다. 인터페이스 빌더에서 화면과 화면을 연결하는 화살표 선이 바로 세그웨이입니다.

<u>Manual Segue</u>

* show

  내비게이션 바로 화면을 옆으로 밀면서 변경할 때

* show detail

  내비게이션 바로 화면을 변경할 때

* present modally

  화면을 전체 바꿀 때(일반적인 화면 전환)

* present as Popover

  팝 오버로 변경할 때

* custom

  사용자 정의 방법을 사용해 변경할 때

<u>Relationship Segue</u>

* view controllers

  탭 바로 화면을 변경할 때

#### 1.2 화면마다 뷰 컨트롤러 만들기

지금까지 만들었던 애플리케이션의 프로그램은 SingleViewContoller 템플릿을 이용하여 화면이 1개였습니다. 때문에 해당 화면에 대응하는 ViewContoller.swift라는 뷰 컨트롤러 1개로 컨트롤 하였습니다. 

그런데 여러 개의 화면을 만들 때는 여러 개의 뷰 컨트롤러가 필요합니다. 따라서 화면을 추가하면 새로운 뷰 컨트롤러도 함께 만들어집니다. 예를 들어 2개의 화면이 있다면 뷰 컨트롤러도 2개 있어야 합니다. 2개의 컨트롤러는 각각 자신의 화면만 담당합니다. 

만약 2개의 화면이 있을때 서로 연계해서 무언가를 해야한다면 뷰 컨트롤러는 서로의 상태를 모르기 때문에 연락 적용 속성을 만들어두고, 여기에 정보를 쓰고나 읽어서 서로 상태를 주고받아야 합니다.

더 많은 화면이 만들어지면 연락하는 방법도 복잡해지기 때문에 이러한 때에는 앱 델리게이트(AppDelegate)를 사용해서 연락하는 것이 좋습니다. 앱 델리게이트에 연락 전용 속성을 만들어 두고, 이를 사용해 다른 뷰 컨트롤러들이 속성을 **공유**해서 사용하는 것입니다. 이렇게 하면 여러개의 뷰 컨트롤러끼리 정보를 쉽게 주고받을 수 있습니다.

델리게이트란 예를 들어 테이블 뷰에서 리스트를 출력할 때, 델리게이트를 사용해서 뷰 컨트롤러에게 무엇을 해야하는지 묻습니다. '이건 몇 번째 줄에 출력할까요?'와 같은 질문을 받으면 뷰 컨트롤러는 '첫 번째 줄에 출력해주세요'와 같이 지시합니다. 

> 1개의 화면은 1개의 뷰 컨트롤러에 맡김
>
> 여러 개의 화면이 있다면 뷰 컨트롤러도 여러 개
>
> 델리게이트: 뷰 컨트롤러에게 무엇을 어떻게 할지 지시

#### 1.3 데이터 연동 방법

화면이 여러 개 있는 애플리케이션에서 각 화면은 서로 다른 뷰 컨트롤러가 제어하므로 서로 간의 정보를 알 수 없습니다. 따라서 화면을 서로 연동하려면 데이터를 주고 받거나 공유해야 합니다.

데이터를 연동하는 방법에는 크게 2가지 방법이 있습니다.

<u>방법1: 직접 데이터를 주고받는 방법</u>

* 다음 화면으로 변경할 때, 데이터를 전달
* 원래 화면으로 돌아올 때, 다음 화면에서 데이터를 받음

<u>방법2: 앱 델리게이트를 사용해서 데이터를 공유하는 방법</u>

* 앱 델리게이트에 속성을 생성
* 처음 화면에서 앱 델리게이트의 속성에 데이터를 저장
* 다음 화면에서 앱 델리게이트의 속성에서 데이터를 읽음

### 2. 싱글 뷰 애플리케이션 예제

#### 2.1 새로운 화면 추가 과정

싱글 뷰 애플리케이션에 새로운 화면을 추가할 때는 다음과 같은 과정을 거칩니다.

> ​	[1] 인터페이스 빌더에서 새로운 화면을 추가
>
> ​	[2] 새로운 화면 전용 뷰 컨트롤러를 만들고 새로운 화면에 설정
>
> ​	[3] 세그웨이를 사용해서 첫 번째 화면과 새로운 화면을 연결
>
> ​	[4] 첫 번째 화면에 돌아올 수 있는 문 생성(unwind segue)
>
> ​	[5] 돌아올 수 있는 문을 사용해 새로운 화면과 첫 번째 화면을 연결

#### 2.2 [튜토리얼] 색상을 전달하는 애플리케이션 만들기

<u>어떤 애플리케이션?</u>

화면이 2개인 애플리케이션 입니다. 첫 번째 화면에는 RGB 값과 색상 보기라는 부품이 있고, 색상 보기 버튼을 탭하면 두 번째 화면으로 변경됩니다. 이때 두 번째 화면의 배경에 RGB 값에 있던 색을 칠하여 보여줍니다.

<u>소스 설명</u>

다른 화면에서 특정 화면으로 돌아오려면 메서드가 필요합니다. 메서드를 만들기만 하면 자동으로 돌아올 수 있는 문이 만들어지며 이를 unwind segue라고 부릅니다.

화면 1에서 화면2로 변경하기 직전에 prepare() 메서드가 호출됩니다. 이 메서드의 segue.destination이 변경할 대상의 뷰 컨트롤러 입니다. 이를 as! ColorViewController로 변환하여 colorR, colorG, colorB 값을 전달합니다.

> @IBAction fund <메서드 이름> (segue: UIStoryboardSegue)
>
> Override func <메서드 이름>(for segue: UIStoryboardSegue, sender: Any?){}

```swift
/*ViewController.swift*/
import UIKit
//랜덤 메서드 사용 준비
import GameplayKit

class ViewController: UIViewController{
  @IBOutlet weak var colorLabel: UILabel!
  
  //랜덤 변수
  let randomSource = GKARC4RandomSource()
  var colorR = 0
  var colorG = 0
  var colorB = 0
  
  //화면을 출력할 때 호출되는 메서드 오버라이드
  override func viewWillApper(_ animated: Bool){
    //0~255 범위의 랜덤한 숫자 3개를 구함
    colorR = randomSource.nextInt(upperBound: 256)
    colorG = randomSource.nextInt(upperBound: 256)
    colorB = randomSource.nextInt(upperBound: 256)
    
    //3개의 값을 출력
    colorLabel.text = "R=\(colorR), G=\(colorG), B=\(colorB)"
  }
  
  override func viewDidLoad(){
    super.viewDidLoad()
  }
  
  override func prepare(for segue: UIStoryboardSegue, sender: Any?){
    //변경될 화면 추출
    let nextvc = segue.destination as! ColorViewController
    
    //변경될 화면에 있는 변수에 값을 전달
    nextvc.colorR = colorR
    nextvc.colorG = colorG
    nextvc.colorB = color
  }
  
  @IBAction func returnTop(segue: UIStoryboardSegue){
    
  }
  
}
```

ColorViewController.swift를 클릭해서 화면 2의 프로그램을 엽니다.

RGB 값을 전달 받을 수 있는 변수를 생성하고 초기값은 0으로 설정합니다. 그리고 화면을 출력할 때 호출되는 viewWillAppear() 메서드에서 3개의 값을 사용해 배경색을 설정합니다.

추가로 UIColor는 부동 소수점으로 Double이 아니라 CGFloat 자료형을 사용해야 합니다.

```swift
/*ColorViewController*/
import UIKit

class ColorViewController: UIViewController {
  //RGB 값을 나타내는 변수를 선언
  var colorR = 0
  var colorG = 0
  var colorB = 0
  
  //화면을 출력할 때 호출되는 메서드
  override func viewWillAppear(_ animated: Bool){
    //RGB 값을 기반으로 색상을 만듬
    let backColor = UIColor(
    red: CGFloat(colorR)/256.0,
    green: CGFloat(colorG)/256.0,
    blue: CGFloat(colorB)/256.0,
    alpha: 1.0)
    //만든 색상을 배경 색상으로 설정
    view.backgroundColor = backColor
  }
  .
  .
  .
}
```

### 3. 탭 기반 애플리케이션 예제

Tabbed Application 템플릿을 이용하여 여러 화면을 전환하는 애플리케이션 입니다.

#### 3.1 화면 간 데이터 공유 과정

> ​	[1] 예제 속 화면1, 화면2, 화면3의 텍스트 필드와 버튼을 연결
>
> ​	[2] AppDelegate.swift에 공유할 데이터를 변수로 생성
>
> ​	[3] 화면1에서 공유해서 사용하는 데이터를 출력하고, 값을 입력하면 변수를 변경
>
> ​        [4] 화면2와 화면3에서 공유해서 사용하는 데이터 출력, 값을 입력하면 변수를 변경

#### 3.2 [튜토리얼] 단위 환산 애플리케이션

<u>어떤 애플리케이션?</u>

처음 화면에서 cm 단위를 입력하면 다른 탭에 inch와 척 단위로 환산된 결과를 출력합니다.

<u>소스코드 설명</u>

3개의 화면에서 같은 값을 공유하여 사용할 수 있게 앱 델리게이트 변수를 만듭니다. 단위를 환산하면 부동 소수점이 나올 수 있으므로 Double 자료형을 사용합니다.

```swift
/* AppDelegate.swift */
import UIKit
@UIApplication
class AppDelegate: UIResponder, UIApplicationDelegate{
  var window: UIWindow?
  
  //공유할 변수
  var cmValue:Double = 1.0
  
  func application(_ application: UIApplication, didFinishLaunchingWithOption launchOptions: [NSObject: AnyObject?]) -> Bool {
   return true 
  }
  .
  .
  .
  소스코드 생략
}
```

앱 델리게이트에 접근하려면 앱 델리게이트 객체를 추출해야 합니다. 화면을 출력할 때 앱 델리게이트 객체를 추출하고, 공유를 위해 만든 변수의 값을 텍스트 필드에 출력합니다.

> let <AppDelegate 객체> = UIApplication.shared.delegate as! AppDelegate

```swift
/* FirstViewController.swift */
import UIKit

class FistViewController: UIViewController {
  @IBOutlet weak var dataTextField: UITextField!
  
  //AppDelegate에 접근할 수 있게 객체를 만듭니다.
  let ap =  UIApplication.shared.delegate as! AppDelegate
  
  //화면을 출력할 때 호출되는 메서드
  override func viewWillAppear(_ animated: Bool){
    //공유 변수의 값을 텍스트 필드에 설정
    dataTextField.text = String (ap.cmValue)
  }
  
  @IBAction func tapInput(){
    //키보드를 닫기
    dataTextField.resignFirstResponder()
    if let text = dataTextField.text{
      //텍스트 필드에 값이 있을 때
      if let cmValue = Double(text){
        //부동 소수 값으로 변환할 수 있다면, 공유 변수에 값을 적습니다
        ap.cmValue = cmValue
      }
    }
  }
}
```

화면 2 또는 화면 3을 출력할 때, 공유 변수를 텍스트 필드에 출력합니다. 

화면 2를 출력할 때는 cm을 inch로 변환해서 텍스트 필드에 출력하고, 화면 3을 출력할 때는 cm을 척으로 변환해서 출력합니다. 그리고 사용자가 입력 버튼을 누르면 화면 2와 화면 3에 있는 텍스트 필드의 값을 각각 cm으로 변환해서 앱 델리게이트의 공유 변수에 집어넣습니다.

> let <inch 값> = <cm 값> * 0.3937

```swift
/* SecondViewController.swift */
import UIKit

class SecondViewController: UIViewController {
  @IBOutlet weak var dataTextField: UITextField!
  
  //AppDelegate에 접근할 수 있게 객체를 만듭니다.
  let ap =  UIApplication.shared.delegate as! AppDelegate
  
  //화면을 출력할 때 호출되는 메서드
  override func viewWillAppear(_ animated: Bool){
    //공유 변수의 값을 텍스트 필드에 설정
    let inchValue = ap.cmValue * 0.3937
    dataTextField.text = String(inchValue)
  }
  
  @IBAction func tapInput(){
    //키보드를 닫기
    dataTextField.resignFirstResponder()
    if let text = dataTextField.text{
      //텍스트 필드에 값이 있을 때
      if let inchValue = Double(text){
        //부동 소수 값으로 변환할 수 있다면, 공유 변수에 값을 적습니다
        ap.cmValue = inchValue / 0.3937
      }
    }
  }
}
```

```swift
/* ThirdViewController.swift */
import UIKit

class SecondViewController: UIViewController {
  @IBOutlet weak var dataTextField: UITextField!
  
  //AppDelegate에 접근할 수 있게 객체를 만듭니다.
  let ap =  UIApplication.shared.delegate as! AppDelegate
  
  //화면을 출력할 때 호출되는 메서드
  override func viewWillAppear(_ animated: Bool){
    //공유 변수의 값을 텍스트 필드에 설정
    let sunValue = ap.cmValue * 0.33
    dataTextField.text = String(sunValue)
  }
  
  @IBAction func tapInput(){
    //키보드를 닫기
    dataTextField.resignFirstResponder()
    if let text = dataTextField.text{
      //텍스트 필드에 값이 있을 때
      if let sunValue = Double(text){
        //부동 소수 값으로 변환할 수 있다면, 공유 변수에 값을 적습니다
        ap.cmValue = sunValue / 0.33
      }
    }
  }
}
```



---

### 참고문헌

* '스위프트로 시작하는 아이폰 앱 개발 교과서', 모리 요시나오, 위키북스
