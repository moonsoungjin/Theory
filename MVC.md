# MVC 패턴

<img src = "https://media.discordapp.net/attachments/1024844158386577458/1055004120450342942/2022-12-21_3.10.02.png">

<br>

MVC 패턴은 애플에서 기본적으로 지원하는 디자인 패턴으로, **Model + View + Controllor** 구조의 아키텍처 패턴을 말한다.
- model: 앱의 데이터와 비지니스 로직을 갖고 있다.
- view: 사용자에게 데이터를 보여주거나 UI를 담당한다.
- controller: model과 view의 중간다리 역할로 view로부터 사용자의 action을 받아 model에게 어떤 작업을 해야 하는지 알려주거나, model의 데이터 변화를 view에게 전달하여 view를 어떻게 업데이트할지 알려준다.

<br>

### MVC 패턴에서 각 영역은 어떻게 대화할까?
Controller는 Model과 View에 직접 지시를 할 수 있지만 Model과 View는 Controller에 직접적으로 알릴 수 없다.
그렇다면 만약 Model의 데이터가 변경된 것을 알리거나, View에서 사용자의 action이 발생하였을 때 Controller에 어떻게 알릴까?

<br>

### View to Controller

<img src = "https://media.discordapp.net/attachments/1024844158386577458/1055007937501876294/2022-12-21_3.25.18.png">

- Controller는 View에서 발생할 수 있는 action에 대한 target을 만들어둔다.
- View에서 유저의 action이 발생할 경우 Controller에 있는 target이 이를 받아들이고 작업을 수행한다.
- View는 delegate 패턴의 delegate와 datasource를 이용하여 Controller에게 어떤 작업을 수행해야하는지 알리기도 한다.
- 대표적인 예로 UITableView의 UITableViewDelegate와 UITableViewDatasource를 들 수 있다.

### Model to Controller

<img src = "https://media.discordapp.net/attachments/1024844158386577458/1055009351141699594/2022-12-21_3.30.56.png">

- Model은 Observer 패턴의 Notification과 KVO를 통해 Controller에게 알린다.
- Notification과 KVO는 일을 수행하는 객체(publisher)가 진행하던 작업이 끝나면 자신들을 구독 중인 객체들(subscribers)에게 신호를 보내는 방식이다.
- 간단하게 설명하면, 작업이 완료됐을 때 라디오 센터에서 전파를 보내는 것처럼 Controller에게 신호를 보낸다.

<br>

### MVC 패턴 예시

- Model
```swift
import Foundation

struct User {
    let name: String
    let age: Int
    let job: String
}
```
유저의 이름,나이,직업 데이터를 저장할 User 구조체 생성

<br>

- View + Controller
```swift
import UIKit

class UserCell: UITableViewCell {
    @IBOutlet week var nameLabel: UILabel!
    @IBOutlet week var ageLabel: UILabel!
    @IBOutlet week var jobLabel: UILabel!
}

class UserListViewController: UIViewController {
    
    @IBOutlet week var tableView: UITableView!

    override func viewDidLoad() {
        super.viewDidLoad() {
            tableView.dataSource = self
            tableView.delegate = self
        }

    let users: [User] = [
        User(name: "sungjin", age: 19, job: "student")
        User(name: "taemin", age: 19, job: "student")
        User(name: "seyoon", age: 19, job: "student")
        User(name: "yejun", age: 19, job: "student")
     ]
}

extension UserListViewController: UITableViewDataSource {
    func tableView(_ tableView: UITableView, numberOfRowsInSection section: Int) -> Int {
        return users.count
    }

    func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell {
        guard let cell = tableView.dequeueReusbleCell(withIdentifier: "UserCell", for: indexPath) as? UserCell else {
            return UITableViewCell()
        }

        cell.nameLabel.text = users[indexPath.row].name
        cell.ageLabel.text = "(\(users[indexPath.row].age))"
        cell.jobLabel.text = users[indexPath.row].job

        return cell
    }
}

extension UserListViewController: UITableViewDelegate {
    func tableView(_ tableView: UITableView, heightForRowAt indexPath: IndexPath) -> CGFloat{
        return CGFloat(100)
    }
}
```

맨 위에 있는 UserCell은 UITableViewCell을 상속 받는 tableview의 cell에 보여줄 View를 담당한다
UserCell을 통해 유저의 이름, 나이, 직업을 보여줄 수 있고, ViewController에 보이는 tableView도 View를 담당한다
나머지 부분은 Controller를 담당하며, 여기에는 View의 Life Cycle도 포함된다
밑에 보이는 UITableViewSource와 UITableViewDelegate를 통해 데이터를 이용하여 View를 어떻게 보여줄지 구현해놓은 부분을 볼 수 있다

<br>

### MVC 패턴의 장점 및 단점

#### 장점
- 다른 패턴에 비해 코드량이 적다
- 애플에서 기본적으로 지원하고 있는 패턴이기 때문에 쉽게 접근할 수 있다
- 개발자들이 쉽게 유지보수 할 수 있다
- 개발 속도가 빠르기 때문에 아키텍처가 중요하지 않을 때 사용하거나 규모가 작은 프로젝트에서 사용하기 좋다

#### 단점

<img src = "https://media.discordapp.net/attachments/1024844158386577458/1055076284205629480/2022-12-21_7.56.50.png">

- View와 Controller가 너무 밀접하게 연결되어 있다
- ViewController에서 볼 수 있듯이, View와 Controller가 붙어 있으며, Controller가 View의 Life Cycle까지 관리하기 때문에 View와 Controller를 분리하기 어렵다
- 재사용성이 떨어지고, 유닛 테스트를 진행하기 힘어진다
- 대부분의 코드가 Controller에 밀집될 수 있다