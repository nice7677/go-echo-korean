## 커스텀마이징

### 컨텍스트
`echo.Context`는 현재 HTTP 요청의 컨텍스트에 해당합니다.
요청 및 응답 참조, 경로, 경로 매개 변수, 데이터, 등록 된 핸들러 및 요청을 읽고 응답을 쓰기위한 API를 보유하고 있습니다.
컨텍스트는 인터페이스이므로 커스텀 API로 쉽게 확장 할 수 있습니다.

### 컨텍스트 확장

**커스텀 컨텍스트 정의**
```go
type CustomContext struct {
	echo.Context
}

func (c *CustomContext) Foo() {
	println("foo")
}

func (c *CustomContext) Bar() {
	println("bar")
}
```

기본 컨텍스트를 확장하는 미들웨어를 만들기
```go
e.Use(func(next echo.HandlerFunc) echo.HandlerFunc {
	return func(c echo.Context) error {
		cc := &CustomContext{c}
		return next(cc)
	}
})
```
> 이 미들웨어는 다른 미들웨어보다 먼저 등록해야합니다.

핸들러에서 사용하기
```go
e.GET("/", func(c echo.Context) error {
	cc := c.(*CustomContext)
	cc.Foo()
	cc.Bar()
	return cc.String(200, "OK")
})
```