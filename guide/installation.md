## 설치

### 조건
- Go 설치
- GOPATH 설정

#### go get을 사용해 설치하기
```shell
$ cd <PROJECT IN $GOPATH>
$ go get -u github.com/labstack/echo/...
```
#### dep를 사용해 설치하기
```shell
$ cd <PROJECT IN $GOPATH>
$ dep ensure -add github.com/labstack/echo@^3.1
```
#### glide를 사용해 설치하기
```shell
$ cd <PROJECT IN $GOPATH>
$ glide get github.com/labstack/echo#~3.1
```
#### govendor를 사용해 설치하기
```shell
$ cd <PROJECT IN $GOPATH>
$ govendor fetch github.com/labstack/echo@v3.1
```