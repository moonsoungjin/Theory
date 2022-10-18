# Dynamic Type
- Dynamic Type은 사용자가 원하는 글씨 사이즈로 App의 contents를 볼 수 있는 적응성(flexibility)을 제공한다

### Accessibility Inspector란?
- Accessibility Inspector는 접근성을 검사해 볼 수 있는 tool이다
- 여기서는 앱의 font의 사이즈를 변경해 볼 수도 있고, 시각장애인들을 위한 VoiceOver를 테스트 해 볼 수도 있다

<img src="https://media.discordapp.net/attachments/1024844158386577458/1031389737220583554/2022-10-17_11.13.29.png">

xcode -> Open Developer Tool -> Accessibility Insepctor을 누르면 아래와 같은 창이 생긴다

<img src="https://media.discordapp.net/attachments/1024844158386577458/1031389726906789908/2022-10-17_11.13.57.png">

이떄 프로젝트를 실행시키면 Simulator를 설정할 수 있고 알 맞은 Simulator로 설정해주면 된다
지금은 font size의 변경에 대해서 테스트 해 볼 것이므로 아래의 그림의 빨간 박스부분을 눌러주면 된다

<img src="https://media.discordapp.net/attachments/1024844158386577458/1031389724545396816/2022-10-17_11.14.14.png">

위의 그림처럼 Font size를 조절할 수 있는 silder를 볼 수 있다

### Dynamic Type 의 필요성
- Dynamic Type이 적용된 UILabel과 그렇지 않은 UILabel의 차이를 확인해 보자

<img src="https://media.discordapp.net/attachments/1024844158386577458/1031389848549982339/2022-10-17_11.15.10.png">

Dynamic Type이 설정되어 있지 않은 UILabel의 경우에는 Font size가 커져도 글자의 사이즈가 그대로인 반면에 Dynamic Type이 적용된 UILabel은 글씨가 커진 것을 확인할 수 있다

즉, Dynamic Tyep이 적용되지 않은 앱에서는 사용자가 설정에서 글씨의 크기를 키워도 App contents의 크기는 커지지 않게 된다<br>
따라서 Dynamic Type을 적용해주는 HIG를 중요시 여기는 Apple의 관점에서는 매우 중요하다<br>
또한 모든 사용자를 배려하는 앱을 만들기 위해서는 필수적으로 적용시켜야 할 것 같다는 생각이 든다

### Dynamic Type 적용하기
- Dynamic Type을 적용하는 방법은 code를 통해서 적용하거나 Interface Builder를 통해서 적용할 수 있다

#### Interface Builder
<img src="https://media.discordapp.net/attachments/1024844158386577458/1031389925746163803/2022-10-17_11.15.28.png">

Interface Builder를 통해서 Dynamic Type을 정의하는 방법은 위의 그림과 같다
UILabel의 경우에는 이렇게 Dynamic Type을 적용시킬 수 있지만 button안에 있는 글자들은 Interface Builder에서 font는 설정해 줄 수 있지만 Dynamic Type을 적용시킬 수 없다 이부분은 코드로 작성해야한다

#### code

```
import UIKit

class ViewController: UIViewController {
    @IBOutlet weak var label: UILabel!
    
    override func viewDidLoad() {
        super.viewDidLoad()
        
        label.font = UIFont.preferredFont(forTextStyle: .title3)
        label.adjustsFontForContentSizeCategory = true
    }
}
```

- label의 font를 prefferedFont()를 통해서 정해주고
- adjustsFontForContentSizeCategory = true로 설정하는 부분은 위의  Interface Builder에서는 Automatically adjusts Font에 check를 해주는 부분이다

button

```
import UIKit

class ViewController: UIViewController {
    
    @IBOutlet weak var button: UIButton!
    @IBOutlet weak var label: UILabel!
    override func viewDidLoad() {
        super.viewDidLoad()
        
        label.font = UIFont.preferredFont(forTextStyle: .title3)
        label.adjustsFontForContentSizeCategory = true
        
        NotificationCenter.default.addObserver(self, selector: #selector(adjustButtonDynamicType), name: UIContentSizeCategory.didChangeNotification, object: nil)
    }
    
    @objc func adjustButtonDynamicType() {
        button.titleLabel?.adjustsFontForContentSizeCategory = true
    }
}

```

- 위의 코드에서 button을 추가해주었다. 그리고 NotificationCenter를 사용해서 UIContentSizeCategory 의 값이 바뀌게 되면 adjustButtonDynamicType() 함수를 실행주도록 한다
- 이때 adjustButtonDynamicType() 함수 안에서는 adjustsFontForContentSizeCategory = true 로 설정해주게 되면 button 안의 글자들에도 Dynamic Type을 적용할 수 있다