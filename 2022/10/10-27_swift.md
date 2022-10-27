# 2022-10-27 Swift

## 함수

### 함수 기본

> 함수선언의 기본형태

```swift
func 함수이름(매개변수1이름: 매개변수1타입, 매개변수2이름: 매개변수2타입 ...) -> 반환타입 {
    /* 함수 구현부 */
    return 반환값
}

// 예시)
// sum이라는 이름을 가지고
// a와 b라는 Int타입의 매개변수를 가지며
// Int타입의 값을 반환하는 함수
func sum(a: Int, b: Int) -> Int {
    return a + b
}
```

> 반환 값이 없는 함수

```swift
func 함수이름(매개변수1이름: 매개변수1타입, 매개변수2이름: 매개변수2타입 ...) -> void {
    /* 함수 구현부 */
    return
}

// 예시)
func printMyName(name: String) -> void {
    print(name)
}

// 반환값이 없는 경우, 반환 타입(void)을 생략해 줄 수 있습니다.
func printYoutName(name: String) {
    print(name)
}
```

> 매개변수가 없는 함수

```swift
func 함수이름() -> 반환타입 {
    /* 함수 구현부 */
    return 반환값
}

// 예시)
func maximumIntegerValue() -> Int {
    return Int.max
}
```

> 매개변수와 반환값이 없는 함수

```swift
func 함수이름() -> Void {
    /* 함수 구현부 */
    return
}

// 함수 구현이 짧은 경우
// 가독성을 해치지 않는 범위에서
// 줄바꿈을 하지 않고 한 줄에 표현해도 무관합니다
func hello() -> Void { print("hello") }

// 반환 값이 없는 경우, 반환 타입(Void)을 생략해 줄 수 있습니다
func 함수이름() {
    /* 함수 구현부 */
    return
}

// 예시)
func bye() { print("bye") }
```

> 함수의 호출

```swift
sum(a: 3, b: 5) // 8
printMyName(name: "yagom") // yagom
printYourName(name: "hana") // hana
maximumIntegerValue() // Int의 최댓값
hello() // hello
bye() // bye
```

---
출처 : 야곰 닷넷 - 스위프트 기초 [함수 기본](https://yagom.net/courses/swift-basic/lessons/%ed%95%a8%ec%88%98/topic/%ed%95%a8%ec%88%98-%ea%b8%b0%eb%b3%b8/)
___

### 함수 고급

**매개변수 기본 값**

매개변수에 기본적으로 전달될 값을 미리 지정할 수 있습니다.

> 기본값을 갖는 매개변수는 목록 중에 뒤쪽에 위치하는 것이 좋습니다.

```swift
func 함수이름(매개변수1이름: 매개변수1타입, 매개변수2이름: 매개변수2타입 = 매개변수 기본값 ...) -> 반환타입 {
    /* 함수 구현부 */
    return 반환값
}

func greeting(friend: String, me: String = "yagom") {
    print("Hello \(friend)! I'm \(me)")
}

// 매개변수 기본값을 가지는 매개변수는 호출시 생략할 수 있습니다
greeting(friend: "hana") // Hello hana! I'm yagom
greeting(friend: "john", me: "eric") // Hello john! I'm eric
```

**전달인자 레이블**

* 함수를 호출할 때 함수 사용자의 입장에서 매개변수의 역할을 좀 더 명확하게 표현하고자 할 때 사용합니다.

```swift
func 함수이름(전달인자 레이블 매개변수1이름: 매개변수1타입, 전달인자 레이블 매개변수2이름: 매개변수2타입 ...) -> 반환타입 {
    /* 함수 구현부 */
    return
}

// 함수 내부에서 전달인자를 사용할 때에는 매개변수 이름을 사용합니다
func greeting(to friend: String, from me: String) {
    print("Hello \(freind)! I'm \(me)")
}

// 함수를 호출할 때에는 전달인자 레이블을 사용해야 합니다
greeting(to: "hana", from: "yagom" )
// Hello hana! I'm yagom
```

**가변 매개변수**

전달 받을 값의 개수를 알기 어려울 때 사용할 수 있습니다

> 스위프트 버전 5.4미만에서는 함수당 하나의 가변 매개변수만 가질 수 있습니다.(5.4 버전부터 여러개 가능)

```swift
func 함수이름(매개변수1이름: 매개변수1타입, 전달인자 레이블 매개변수2이름: 매개변수2타입...) -> 반환타입 {
    /* 함수 구현부 */
    return
}

func sayHelloToFriends(me: String, friends: String...) -> String {
    return "Hello \(friends)! I'm \(me)!"
}
print(sayHelloToFriends(me: "yagom", freinds: "hana", "eric", "wing"))
// Hello ["hana", "eric", "wing"]! I'm yagom!

print(sayHelloToFriends(me: "yagom"))
// Hello []! I'm yagom!
```

> 위에 설명한 함수의 다양한 모양은 모두 섞어서 사용 가능합니다

**데이터 타입으로서의 함수**

스위프트는 함수형 프로그래밍 패러다임을 포함하는 다중 패러다임 언어이므로 스위프트의 함수는 일급객체입니다. 그래서 함수를 변수, 상수 등에 할당이 가능하고 매개변수를 통해 전달할 수 있습니다

**함수의 타입표현**

반환타입을 생략할 수 없습니다

    (매개변수1타입, 매개변수2타입 ...) -> 반환타입

**함수타입 사용**

```swift
var someFunction: (String, String) -> Void = greeting(to:from:)
someFunction("eric", "yagom") // Hello eric! I'm yagom

someFunction = greeting(friend:me:)
someFunction("eric", "yagom") // Hello eric! I'm yagom


// 타입이 다른 함수는 할당할 수 없습니다 - 컴파일 오류 발생
//someFunction = sayHelloToFriends(me: friends:)


func runAnother(function: (String, String) -> Void) {
    function("jenny", "mike")
}

// Hello jenny! I'm mike
runAnother(function: greeting(friend:me:))

// Hello jenny! I'm mike
runAnother(function: someFunction)
```

> 참고: 스위프트의 전반적인 문법에서 띄어쓰기는 신경써야할 때가 많습니다

---
출처 : 야곰 닷넷 - 스위프트 기초 [함수 고급](https://yagom.net/courses/swift-basic/lessons/%ed%95%a8%ec%88%98/topic/%ed%95%a8%ec%88%98-%ea%b3%a0%ea%b8%89/)
___

## 제어구문

### 조건문

**if-else 구문**

**if-else 구문의 기본 형태**

if만 단독적으로 사용해도 되고, else if, else와 조합해서 사용 가능합니다.
if 뒤의 조건 값에는 Bool타입의 값만 위치해야 하며, 조건 값을 감싸는 소괄호는 선택 사항입니다.

```swift
if 조건 {
    /* 실행 구문 */
} else if 조건 {
    /* 실행 구문 */
} else {
    /* 실행 구문 */
}
```

**if-else의 사용**

```swift
let someInteger = 100

if someInteger < 100 {
    print("100 미만")
} else if someInteger > 100 {
    print("100 초과")
} else {
    print("100")
} // 100

// 스위프트의 조건에는 항상 Bool 타입이 들어와야합니다
// someInteger는 Bool 타입이 아닌 Int 타입이기 때문에
// 컴파일 오류가 발생합니다
//if someInteger { }
```

**switch 구문**

스위프트의 swirch구문은 다른 언어에 비해 굉장히 강력한 힘을 발휘합니다. 기본적으로 사용하던 정수타입의 값만 비교하는 것이 아니라 대부분의 스위프트 기본 타입을 지원하며, 다양한 패턴과도 응용이 가능합니다. 스위프트의 다양한 패턴은 [Swift Programming Language Reference의 패턴](https://docs.swift.org/swift-book/ReferenceManual/Patterns.html#//apple_ref/doc/uid/TP40014097-CH36-ID419)에서 확인할 수 있습니다

> 각각의 case 내부에는 실행가능한 코드가 반드시 위치해야 합니다
> 매우 한정적인 값(ex. enum의 case 등)이 비교값이 아닌 한 default 구문은 반드시 작성해야 합니다
> 명시적 break를 하지 않아도 자동으로 case마다 break 됩니다
> fallthrough 키워드를 사용하여 break를 무시할 수 있습니다.
> 쉼표(,)를 사용하여 하나의 case에 여러 패턴을 명시할 수 있습니다.

**switch 구문의 기본 형태**

```swift
switch 비교값 {
case 패턴:
    /* 실행 구문 */
default:
    /* 실행 구문 */
}
```

**switch 구문의 사용**

```swift
// 범위 연산자를 활용하면 더욱 쉽고 유용합니다
switch someInteger {
case 0:
    print("zero")
case 1..<100:
    print("1~99")
case 100:
    print("100")
case 101...Int.max:
    print("over 100")
default:
    print("unknown")
} // 100

// 정수 외의 대부분의 기본 타입을 사용할 수 있습니다
switch "yagom" {
case "jake":
    print("jake")
case "mina":
    print("mina")
case "yagom":
    print("yagom!!")
default:
    print("unknown")
} // yagom!!
```

> 기본 문법을 익힌 뒤 차후에 더 많은 switch 구문과 패턴의 활용에 대해 알아봅시다

---
출처 : 야곰 닷넷 - 스위프트 기초 [조건문](https://yagom.net/courses/swift-basic/lessons/%ec%a0%9c%ec%96%b4%ea%b5%ac%eb%ac%b8/topic/%ec%a1%b0%ea%b1%b4%eb%ac%b8/)
___

### 반복문

**for-in 구문**

기존 언어의 for-each 구문과 유사합니다. Dictionary의 경우 이터레이션 아이템으로 튜플이 들어옵니다. 튜플에 관해서는 [Swift Language Guide의 Tuples 부분](https://docs.swift.org/swift-book/LanguageGuide/TheBasics.html)을 참고하면 되겠습니다

**for-in 구문의 기본 형태**

```swift
for item in items {
    /* 실행 구문 */
}
for-in 구문의 사용
var integers = [1, 2, 3]
let people = ["yagom": 10, "eric": 15, "mike": 12]

for integer in integers {
    print(integer)
}

// Dictionary의 item은 key와 value로 구성된 튜플 타입입니다
for (name, age) in people {
    print("\(name): \(age)")
}
```

**while 구문**

**while 구문의 기본 형태**

```swift
while 조건 {
    /* 실행 구문 */
}
while 구문의 사용
while integers.count > 1 {
    integers.removeLast()
}
```

**repeat-while 구문**

기존 언어의 do-while 구문과 형태 및 동작이 유사합니다

**repeat-while 구문의 기본 형태**

```swift
repeat {
    /* 실행 구문 */
} while 조건
repeat-while 구문의 사용
repeat {
    integers.removeLast()
} while integers.count > 0
```

---
출처 : 야곰 닷넷 - 스위프트 기초 [반복문](https://yagom.net/courses/swift-basic/lessons/%ec%a0%9c%ec%96%b4%ea%b5%ac%eb%ac%b8/topic/%eb%b0%98%eb%b3%b5%eb%ac%b8/)
___

### 옵셔널

* 값이 '있을 수도, 없을 수도 있음'
* nil의 가능성을 명시적으로 표현
    * nil 가능성을 문서화 하지 않아도 코드만으로 충분히 표현가능
        * 문서/주석 작성 시간을 절약
    * 전달받은 값이 옵셔널이 아니라면 nil체크를 하지 않더라도 안심하고 사용
        * 효율적인 코딩
        * 예외 상황을 최고화 하는 안전한 코딩

```swift
// someOptionalParam can be nil
func someFunction(someOptionalParam: Int?) {
    // ...
}
// someParam must not be nil
func someFunction(someParam: Int) {
    // ...
}
someFunction(someOptionalParam: nil)
someFunction(someParam: nil)    // 오류 발생
```

**Optional** → enum + general

```swift
enum Optional<Wrapped> : ExpressibleByNilLiteral {
    case none
    case some(Wrapped)
}

let optionalValue: Optional<Int> = nil
let optionalVaule: Int? = nil
```

**! (Implicitly Unwrapped Optional)** → 암시적 추출 옵셔널

```swift
var implicitlyUnwrappedOptionalValue: Int! = 100

switch implicitlyUnwrappedOptionalValue {
case .none:
    print("This Optional variable is nil")
case .some(let value):
    print("Value is \(value)")
}

// 기존 변수처럼 사용 가능
implicitlyUnwrappedOptionalValue = implicitlyUnwrappedOptionalValue + 1

// nil 할당 가능
implicitlyUnwrappedOptionalValue = nil

// 잘못된 접근으로 인한 런타임 오류 발생
//implicitlyUnwrappedOptionalValue = implicitlyUnwrappedOptionalValue + 1
```

**? (Optional)**

```swift
var optionalValue: Int? = 100

switch optionalValue {
case .none:
    print("This Optional variable is nil")
case .some(let value):
    print("Value is \(value)")
}

// nil 할당 가능
optionalValue = nil

// 기존 변수처럼 사용불가 - 옵셔널과 일반 값은 다른 타입이므로 연산불가
//optionalValue = optionalValue + 1
```

### 옵셔널 값 추출

**Optional Binding** - 옵셔널 바인딩

* 옵셔널의 값을 꺼내오는 방법 중 하나
* nil + 안전한 값 추출

```swift
// Optional Binding
func printName(_ name: String) {
    print(name)
}

var myName: String? = nil

//printName(myName)
// 전달되는 값의 타입이 다르기 때문에 컴파일 오류발생
if let name: String = myName {
    printName(name)
} else {
    print("myName == nil")
}


var yourName: String! = nil

if let name: String = yourName {
    printName(name)
} else {
    print("yourName == nil")
}

// name 상수는 if-let 구문 내에서만 사용가능합니다
// 상수 사용범위를 벗어났기 때문에 컴파일 오류 발생
//printName(name)
// ,를 사용해 한 번에 여러 옵셔널을 바인딩 할 수 있습니다
// 모든 옵셔널에 값이 있을 때만 동작합니다
myName = "yagom"
yourName = nil

if let name = myName, let friend = yourName {
    print("\(name) and \(friend)")
}
// yourName이 nil이기 때문에 실행되지 않습니다
yourName = "hana"

if let name = myName, let friend = yourName {
    print("\(name) and \(friend)")
}
// yagom and hana
```

**Force Unwrapping** - 강제 추출

* 옵셔널의 값을 강제로 추출

```swift
printName(myName!) // yagom
myName = nil

//print(myName!)
// 강제추출시 값이 없으므로 런타임 오류 발생
yourName = nil

//printName(yourName)
// nil 값이 전달되기 때문에 런타임 오류발생
```

---
출처 : 야곰 닷넷 - 스위프트 기초 [옵셔널](https://yagom.net/courses/swift-basic/lessons/%ec%98%b5%ec%85%94%eb%84%90/)
___

###### tags: `swift`
