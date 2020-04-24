# learn_go
A Tour of Go https://tour.golang.org/welcome/1
\
페이지를 참고 하여 나름 정리 한다.
\
go 언어 문법을 모던 언어인 swift와 비교 하면 정리 한다.


## package
패스명의 마지막 이름을 package 이름으로 한다.

## import
여러 패키지를 임포트 할때 () 감싸서 표현 한다.
```go
import (
    "fmt"
    "math"
)
```

이렇게 해도 된다.
```go
import "fmt"
import "math"
```

* 비교 swift
```swift
import Foundation
import UIkit
```

## Exported names
대문자로 시작되어야 외부에서 참조 할 수 있다.
\
????

## 기본 자료형
int int8 int16 int32 int64
\
uint unit8 uint16 uint32 uint64 uintptr
\
byte // uint8 alias
\
rune // int32 alias
\
float32 float64
\
complex64 complex128


## 변수
swift 의 변수 선언과 유사 하게 타입을 뒤에 표시 한다.

```go
var x, y, z int
var c, python, java bool

// := 초기화 및 선언
xy := 1
```
* 비교 siwft
```swift
var x: Int
var y: Int
var z: Int

// 초기화
var xy = 1
```



## 함수
리턴 타임을 swift 처럼 뒤에 표시 한다. 
go 언어에서는 함수에 인자 전달이 무조건 복사다.
```go
func add(x int, y int) int {
    return x + y
}
```
* 비교 siwft
```swift
func add(x: Int, y: Int) -> Int {
    return x + y
}
```

같은 타입은 한번에 x, y int 로 표현 할 수 있다.
\
여러 결과를 리턴 할 수 있다.
(swift 의 tuple 리턴 )
```go
func swap(x, y string) (string, string) {
    return y, x
}
```
* 비교 siwft
```swift
func swap(x: String, y:String) -> (String, String) {
    return (y, x)
}
```

### 함수 값
함수도 값이다.
```go
hypot := func(x, y float64) float64 {
        return math.Sqrt(x*x + y*y)
    }
```

### 함수 클로져 (Function closures)
함수는 클로져다.

adder() 함수는 func(int) int 클로져를 반환 한다.
```go
func adder() func(int) int {
    sum := 0
    return func(x int) int {
        sum += x
        return sum
    }
}
```


## 구조체
```go
type Vertex struct {
    X int
    Y int
}
```
* 비교 swift
```swift
struct Vertex {
    var x: Int
    var y: Int
}
```

구조체의 접근은 . 으로 한다.
```go
type Vertex struct {
    X int
    Y int
}

func main() {
    v := Vertex{1, 2}
    v.X = 4
    fmt.Println(v.X)
}
```

구조체 포인터 접근
```go

type Vertex struct {
    X int
    Y int
}

func main() {
    p := Vertex{1, 2}
    q := &p
    q.X = 1e9
    fmt.Println(p)
}
```

구조체 리터럴 {Name: value}
```go
type Vertex struct {
    X, Y int
}

var (
    p = Vertex{1, 2}  // has type Vertex
    q = &Vertex{1, 2} // has type *Vertex
    r = Vertex{X: 1}  // Y:0 is implicit
    s = Vertex{}      // X:0 and Y:0
)
```

## new 함수
new(T) 는 모든 필드가 0이 할당된 T타입의 포인터를 반환 한다.
```
var t *T = new(T)
```
또는
```
t := new(T)
```

```go
type Vertex struct {
    X, Y int
}

func main() {
    v := new(Vertex)
    fmt.Println(v)
    v.X, v.Y = 11, 9
    fmt.Println(v)
}
```

## 슬라이스 (Slices)
슬라이스는 배열의 값을 point , 그리고 배열의 길이를 가지고 있다.
```go
func main() {
    p := []int{2, 3, 5, 7, 11, 13}
    fmt.Println("p ==", p)

    for i := 0; i < len(p); i++ {
        fmt.Printf("p[%d] == %d\n",
            i, p[i])
    }
}
```
### 슬라이스 자르기 
```go
func main() {
    p := []int{2, 3, 5, 7, 11, 13}
    fmt.Println("p ==", p)
    fmt.Println("p[1:4] ==", p[1:4]) //[3 5 7]

    // missing low index implies 0
    fmt.Println("p[:3] ==", p[:3]) // [2 3 5]

    // missing high index implies len(s)
    fmt.Println("p[4:] ==", p[4:]) // [11 13]
}
```

lo 에서 h-1 의 요소를 포함하는 슬라이스
```
s[lo:hi]
```

하나의 요소를 가지는 슬라이스
```
s[lo:lo+1]
```
* 비교 swift
```swift
s[lo:..<lo+1]
```

## 맵
key value 표현
사용하기 전에 make를 명시 해야 한다.
```go
type Vertex struct {
    Lat, Long float64
}

var m map[string]Vertex

func main() {
    m = make(map[string]Vertex)
    m["Bell Labs"] = Vertex{
        40.68433, -74.39967,
    }
    fmt.Println(m["Bell Labs"])
}

```

맵 리터럴
```go
type Vertex struct {
    Lat, Long float64
}

var m = map[string]Vertex{
    "Bell Labs": Vertex{
        40.68433, -74.39967,
    },
    "Google": Vertex{
        37.42202, -122.08408,
    },
}

```

맵 m의 요소를 삽입
```
m[key] = elem
```
맵 m의 값 가져오기
```
elem = m[key]
```
요소 지우기
```
delete(m, key)
```
키의 존재 여부 확인기
```
elem, ok = m[key]
```

* swift 비교
```swift
var dic : [String:Int] = [:]
```
swift 딕셔너리 값추가
```swift
dic.updateValue(1, forkey:"aaa")
```
swift 딕셔너리 값 접근
```
dic[key]
```
swift 딕셔너리 값 삭제
```
dic.removeValue(forKey: "aaa")
```
swift 딕셔너리 모든값 삭제
```
dic.removeAll()
```



















