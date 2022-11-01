# URLSession 
- 앱과 서버 간의 데이터를 주고 받기 위해서는 HTTP 프로토콜을 이용해 데이터를 주고받음
- 앱에서 서버와 통신하기 위해 애플이 제공하는 API
- HTTP를 포함한 몇가지 프로토콜을 지원하고 인증, 쿠키관리, 캐시관리 등을 지원
- IOS 앱에서 네트워킹을 하기 위해 필요한 API

기본적으로 request, response 구조를 가지고 있다

### URLSession 사용 순서
1. Configuration을 결정
2. Session 생성
3. Request에 사용할 url 설정
4. Task 결정 및 작성

#### URLSessionConfiguration
- URLSession을 생성하기 위해서 필요한 요소
- Configuration을 생성할 때는 URLSession 정책에 따라서 default, ephemeral, background 세 가지 형태로 생성
    - Default: 기본 통신으로 디스크 기반 캐싱을 지원한다
    - Ephemeral: 쿠키나 캐시를 저장하지 않을 정책을 가져갈 때 사용한다(ex: Safari의 개인정보보호 모드)
    - Background: 앱이 백그라운드에 있는 상황에서 컨텐츠 다운로드, 업로드를 할 때 사용한다
-  앱이 종료돼도 통신이 이뤄지는 것을 지원하는 세션

#### URLSession Delegate
- 네트워크 중간 과정을 확인할 수 있다(필요에 따라서 사용됨)

#### URLSession Task
- 작업에 따라서 3가지로 나뉜다

#### DataTask
- Data를 받는 작업, Response 데이터를 메모리 상에서 처리하게 된다
- 백그라운드 세션에 대한 지원이 되지 않는다
- URL 요청을 실시하고 완료 시 핸들러를 호출하는 Task 형식
- Task가 실행된 후 핸들러가 실행되기 때문에 탈출 Closure 형태로 받아와야 한다

#### UploadTask
- 파일을 업로드할 때 사용한다

#### DownloadTask
- 파일을 다운로드 받아서 디스크에 쓸 때 사용한다

#### Configuration 결정 및 Session 생성
- Configuration은 기본 통신인 Default 설정값을 이용하였고 이를 통해서 Session을 생성할 수 있습니다
```
let config = URLSessionConfiguration.default
let session = URLSession(configuration: config)
```

#### Request에 사용할 url 설정
- url을 설정하는 방법은 여러 가지가 있다 Component로 접근해도 되고 한 번에 url을 작성하는 방식도 있다

```
var urlComponents = URLComponents(string: "https://itunes.apple.com/search?")!

var mediaQuery = URLQueryItem(name: "media", value: "music")
var entityQuery = URLQueryItem(name: "entity", value: "song")
var termQuery = URLQueryItem(name: "term", value: "Wondergirls")

urlComponents.queryItems?.append(mediaQuery)
urlComponents.queryItems?.append(entityQuery)
urlComponents.queryItems?.append(termQuery)

var requestURL = urlComponents.url!
```

#### Task 
- URL을 이용해서 request했을 때 응답(response)값을 받아오기 때문에 DataTask를 이용한다
- 실제 네트워킹할 url(requestURL)을 인자로 받고 네트워킹 요청을 하고 나서 완료가 되면 수행작업은 Completion Handler에서 작업을 진행한다

```
let dataTask = session.dataTask(with: requestURL) { data, response, error in
    let successRange = 200..<300
    guard error == nil, let statusCode = (response as? HTTPURLResponse)?.statusCode, successRange.contains(statusCode) else {
        return
    }
    
    guard let resultData = data else {
        return
    }
    
    
    do {
        let decoder = JSONDecoder()
        let response = try decoder.decode(Response.self, from: resultData)
        let tracks = response.tracks
        print(tracks.count)
        for track in tracks {
            print("title: \(track.title)")
            print("artistName: \(track.artistName)")
            print("")
        }
    } catch let error {
        print("--> error: \(error.localizedDescription)")
    }
}

dataTask.resume()

```

#### Closure
- Completion Handler : 네트워크를 통해서 데이터를 받아오면 데이터로 하는 행동을 Closure가 수행한다

```
{ data, response, error in
    let successRange = 200..<300
    guard error == nil, let statusCode = (response as? HTTPURLResponse)?.statusCode, successRange.contains(statusCode) else {
        return
    }
    
    guard let resultData = data else {
        return
    }
    
    
    do {
        let decoder = JSONDecoder()
        let response = try decoder.decode(Response.self, from: resultData)
        let tracks = response.tracks
        print(tracks.count)
        for track in tracks {
            print("title: \(track.title)")
            print("artistName: \(track.artistName)")
            print("")
        }
    } catch let error {
        print("--> error: \(error.localizedDescription)")
    }
}

```

이 다음 오브젝트를 생성해 원하는 정보를 JSON형식으로 받아오면 된다