## 커스텀마이징

### 디버그
- `Echo#Debug`는 디버그 모드를 활성화 또는 비활성화하는데 사용할 수 있습니다. 디버그 모드는 로그 레벨을 `DEBUG`로 설정합니다.

### 로깅
- 로깅 기본 포맷은 JSON 형식이며, 헤더를 수정해 변경할 수 있습니다.

#### 로그 헤더

`Echo#Logger.SetHeader(string)`는 로그의 헤더를 설정하는데 사용할 수 있습니다.

기본값
```json
{"time":"${time_rfc3339_nano}","level":"${level}","prefix":"${prefix}","file":"${short_file}","line":"${line}"}
```
_예제_
```go
import "github.com/labstack/gommon/log"

/* ... */

if l, ok := e.Logger.(*log.Logger); ok {
  l.SetHeader("${time_rfc3339} ${level}")
}
```
```
2018-05-08T20:30:06-07:00 INFO info
```
사용가능한 태그
- `time_rfc3339`
- `time_rfc3339_nano`
- `level`
- `prefix`
- `long_file`
- `short_file`
- `line`

#### 로그 출력

`Echo#Logger.SetOutput(io.Writer)`은 로그의 출력 대상을 설정하는데 사용할 수 있으며 기본값은 `os.Stdout` 입니다.

로그를 완전히 비활성화하려면 `Echo#Logger.SetOutput(ioutil.Discard)` 또는 `Echo#Logger.SetLevel(log.OFF)`로 설정하면 됩니다.

#### 로그 레벨

`Echo#Logger.SetLevel(log.Lvl)`은 로거의 로그 레벨을 설정하는데 사용할 수 있습니다.

기본값은 `ERROR`입니다.

로그 레벨에 사용 가능한 값은 다음과 같습니다.

 - `DEBUG`
 - `INFO`
 - `WARN`
 - `ERROR`
 - `OFF`

#### 커스텀 로거

로깅은 `echo.Logger`를 사용하여 사용자 정의 로거를 등록 할 수있는 `Echo#Logger`인터페이스를 사용하여 구현 됩니다.

### 커스텀 서버

`Echo#StartServer()`는 커스텀 서버를 실행하는데 사용할 수 있습니다.

_예제_
```go
s := &http.Server{
  Addr:         ":1323",
  ReadTimeout:  20 * time.Minute,
  WriteTimeout: 20 * time.Minute,
}
e.Logger.Fatal(e.StartServer(s))
```

### Custom HTTP/2 Cleartext Server

`Echo#StartH2CServer()`는 커스텀 HTTP/2 클리어텍스트 서버를 실행하는데 사용할 수 있습니다.

_예제_
```go
import "golang.org/x/net/http2"

s := &http2.Server{
  MaxConcurrentStreams: 250,
  MaxReadFrameSize:     1048576,
  IdleTimeout:          10 * time.Second,
}
e.Logger.Fatal(e.StartH2CServer(":1323", s))

```

### Startup Banner
`Echo#HideBanner`는 시작 배너를 숨기는데 사용할 수 있습니다.

### Custom Listener
`Echo#*Listener`는 커스텀 리스너를 실행하는데 사용할 수 있습니다.

_예제_
```go
l, err := net.Listen("tcp", ":1323")
if err != nil {
  e.Logger.Fatal(err)
}
e.Listener = l
e.Logger.Fatal(e.Start(""))
```

### HTTP/2 비활설화
`Echo#DisableHTTP2`는 HTTP/2 프로토콜을 비활성화 하는데 사용할 수 있습니다.

### Read Timeout
`Echo#*Server#ReadTimeout`은 요청의 읽기 시간이 타임아웃되기 전에 최대 기간을 설정하는데 사용할 수 있습니다.

### Writer Timeout
`Echo#*Server#WriteTimeout`은 응답의 쓰기 시간이 타임아웃되기 전에 최대 기간을 설정하는데 사용할 수 있습니다.

### 유효성 검사
`Echo#Validator`는 요청이 들어온 페이로드에 대한 데이터 유효성 검사를 수행하기위한 유효성 검증을 등록하는 데 사용할 수 있습니다.

~~Learn more~~

### 커스텀 바인더
`Echo#Binder`는 요청이 들어온 페이로드에 대한 커스텀 바인더를 등록하는데 사용할 수 있습니다.

~~Learn more~~

### 렌더러
`Echo#Renderer`는 템플릿 렌더링을 위해 렌더러 등록에 사용할 수 있습니다.

~~Learn more~~

### HTTP Error Handler
`Echo#HTTPErrorHandler`는 커스텀 HTTP 에러 핸들러를 등록하는데 사용할 수 있습니다.

~~Learn more~~