# 의존성 관리도구

### 의존성 관리도구 주요 기능
- Module
    - 의존성을 알려주는 메타데이터로 라이브러리들이 관리됨
- Manifest
    - 명세서 역할
- Lock
    - 설치된 라이브러리의 버전과 의존 구조를 보여줌
    - 라이브러리가 설치된 다음 적힘
- Repository
    - 모듈이 저장된 공간, 보통 github
- Dependency Constraint
    - 모듈의 허용되는 버전, 보통 Manifest 파일에 적혀짐
- Resolution Rule
- 모듈의 적합한 버전을 설치 해줌

|도구|세팅|빌드속도|
|------|---|---|
|CocoaPods|간편함|느림|
|Carthage|복잡함|빠름|
|Swift Package Manager|간편함|느림|
---
### CocaPods
- 간편함
- 라이브러리 설치 후 추가 작업이 없다
- Xcode 세팅을 자동화해서 시간을 절약할 수 있다
- 라이브러리 목록이 중앙화되어 있다
- 디버깅이 편리하다. 라이브러리 내부 코드를 볼 수 있다

#### pod install
- Podfile.lock에 리스트된 팟들에 대해서 지덩된 버전만 다운(새로운 버전 체크하지 않음)
- Podfile.lock에 리스트되지 않은 팟들은 Podfile에 명시된 버전 조건으로 검색하여 다운로드
- 동료와 버전을 맞추기 위해서는 해당 메서드를 사용해야 한다

#### pod update
- Podfile.lock을 확인하지 않고 최신 버전으로 업데이트

### 작동원리
<img src="https://media.discordapp.net/attachments/1024844158386577458/1026393769215537182/2022-10-03_4.21.59.png">

#### Podfile
- 명세서
```
source "custom repo"
source "custom repo2"

platform: ios, '11.0' # 플랫폼 지정
inhibit_all_warnings ! # library에서 발생하는 warning 무시

target 'Example' do 
    use_frameworks! # Static Library 대신, Framework를 사용

    pod 'SnapKit', '~>5.0.0' # 버전지정: 최소 5.0.0 이상 6.0.0 미만 사용

    target 'ExampleTests' do # 중첩됨
        inherit! :search_paths
        pod 'Quick'
        pod 'Nimble'
    end
end
```

- pod 'SnapKit': 최신 버전 의미
- pod 'SnapKit', '5.0.1': 구체적인 버전
- > 0.1: 초과
- >= 0.1: 이상
- < 0.1: 미만
- <= 0.1: 이하
- ~> 0.1.2: 0.1.2 이상, 0.2 미만
- ~> 0.1: 0.1 이상, 1.0 미만
특정 브랜치를 지정해서 pod에 추가할수도 있다

```
로컬 (직접 라이브러리 개발할 때 사용)
pod 'Alamofire', :path => '~/Documents/Alamofire'

마스터 branch 기준
pod 'Alamofire', :git => '[address].git'

특정 branch 기준
pod 'Alamofire', :git => '[address].git', :branch => 'dev'

태그 기준
pod 'Alamofire', :git => '[address].git', :tag => '3.1.1'

커밋 기준
pod 'Alamofire', :'git' => '[address].git', :commit => '0f506fe12'
```

### Carthage
- 자율성
- 중앙화된 라이브러리 목록 없음
- dependency를 binaryy framework로 빌드해 줌
- 프로젝트 세팅은 사용자 몫
- 빌드 속도 단축을 위해 주로 사용

### Swift Package Manager
- Apple이 만든 first party depdendency manager
- 가장 편리함
- 디버깅 편리(원본 소스 볼 수 있음)
- 하지만 지원안하는 라이브러리들이 많음
    - major 라이브러리는 대부분 지원