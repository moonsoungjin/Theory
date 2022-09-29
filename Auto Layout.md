# Auto Layout(오토 레이아웃)

### 오토 레이아웃 이란?
- 제약조건(Constraints)에 따라 뷰 계층 구조에 있는 모든 뷰의 크기와 위치를 동적으로 지정하는것
---

<img src="https://media.discordapp.net/attachments/1024844158386577458/1024844199910178826/2022-09-29_9.44.40.png" width="400" height="300"/>

- view의 frame 좌표를 직접 지정해주는 것이 아니라, 위와 같이 imageView의 위치나 크기를 다른 객체(Safe Area)를 이용해 "상대적"으로 제약을 주는것
- 내 imageView는 어떤 해상도일지라도, 기준 객체(Safe Area)의 왼쪽으론 30만큼, 오른쪽으론 30만큰, 위로는 50만큼 margin이 있어야하고, height의 크기는 200이라는 제약 조건을 주는것
- 제약 조건으로 준 왼쪽 30, 오른쪽 30이 Constraints이다(지금은 height를 직접 지정했지만, 이또한 지정하지 않고 bottom Constraint로 나타낼수 있음)
- 내 위치를 다른 객체로부터 상대적으로 나타내는 것이 Constraints이다

---

<img src ="https://media.discordapp.net/attachments/1024844158386577458/1024865908964990996/2022-09-29_11.11.15.png" width="700" height="300"/>

- 해상도가 변하더라도 기준 객체(Safe Area)로부터 왼쪽으로 30, 오른쪽으로 30, 위로 50 만큼의 margin이 있으며, height가 200이란 제약 조건이 있는 ImageView를 그리는것
- 이렇게 설정할경우, 내가 ImageView의 width를 별도로 지정해주진 않았지만, 이 경우엔 Leading/ Trailing Cosntraints에 의해 해상도 별로 width가 자동으로 지정됨
- 만약 가로의 크기가 100인 해상도일 경우, ImageView는 leading 30 + trailing 30 margin을 뺀 40만큼 width를 가질 것이고, 만약 가로의 크기가 500인 해상도일 경우, ImageView는 leading 30 + trailing 30 margin을 뺀 440만큼 width를 가질것이다

```
iphon 8             = x:50.0, y:100.0, width:275.0, height: 200.0
iphon 12 pro Max    = x:50.0, y:100.0, width:275.0, height: 200.0
ipad                = x:50.0, y:100.0, width:275.0, height: 200.0
```
- 이렇게 Auto Layout을 설정하면 화면의 해상도에 따라 Frame이 동적으로 달라질 수 있다 

ImageView의 width가 Constraint에 의해 동적으로 지정된다

<img src ="https://media.discordapp.net/attachments/1024844158386577458/1024989041332060220/2022-09-29_7.20.16.png">

만약 이렇게 width를 또 직접 지정하는 Constraint를 추가해버리면, 

<img src ="https://media.discordapp.net/attachments/1024844158386577458/1024990170744881203/2022-09-29_7.25.02.png">

Constraints가 충돌했다며, leding/ trailing/ width 중 하나를 지우라는 에러가 뜬다
Constraints로 인해 width가 동적으로 결정 되는데, 이를 따를 것인지 200이란 숫자를 따를것인지 몰라서 생기는 에러다

### Top / Bottom / Left & Leading / Right & Trailing
<br> Auto Layout을 설정할때
- Top, Bottom은 위쪽, 아래쪽의 제약을 설정하는것
- Left, Right 또한 왼쪽, 오른쪽 제약을 설정하는것

보통은 Left / Right 대신 Leading / Trailing을 왼쪽 오른쪽처럼 사용
하지만 Leading과 Trailing은 왼쪽/오른쪽 개념이 아니다
- Leading: Text가 시작되는 지점
- Trailing: Text가 끝나는 지점