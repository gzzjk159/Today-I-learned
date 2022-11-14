# 2022-11-14 Swift

## 타입 확장

### 프로토콜

프로토콜(Protocol) 은 특정 역할을 수행하기 위한 메서드, 프로퍼티, 기타 요구사항 등의 청사진을 정의합니다. 구조체, 클래스, 열거형은 프로토콜을 **채택(Adopted)** 해서 특정 기능을 수행하기 위한 프로토콜의 요구사항을 실제로 구현할 수 있습니다. 어떤 프로토콜의 요구사항을 모두 따르는 타입은 그 **프로토콜을 준수한다(Conform)** 고 표현합니다. 타입에서 프로토콜의 요구사항을 충족시키려면 프로토콜이 제시하는 청사진의 기능을 모두 구현해야 합니다. 즉, 프로토콜은 기능을 정의하고 제시 할 뿐이지 스스로 기능을 구현하지는 않습니다.

**정의 문법**

protocol 키워드를 사용하여 정의합니다.

```swift
protocol 프로토콜 이름 {
    /* 정의부 */
}
```

**프로토콜 구현**

```swift
protocol Talkable {
    // 프로퍼티 요구
    // 프로퍼티 요구는 항상 var 키워드를 사용합니다
    // get은 읽기만 가능해도 상관 없다는 뜻이며
    // get과 set을 모두 명시하면
    // 읽기 쓰기 모두 가능한 프로퍼티여야 합니다
    var topic: String { get set }
    var language: String { get }
    // 메서드 요구
    func talk()
    // 이니셜라이저 요구
    init(topic: String, language: String)
}
```

**프로토콜 채택 및 준수**

```swift
// Person 구조체는 Talkable 프로토콜을 채택했습니다
struct Person: Talkable {
    // 프로퍼티 요구 준수
    var topic: String
    let language: String
    // 읽기전용 프로퍼티 요구는 연산 프로퍼티로 대체가 가능합니다
//    var language: String { return "한국어" }
    // 물론 읽기, 쓰기 프로퍼티도 연산 프로퍼티로 대체할 수 있습니다
//    var subject: String = ""
//    var topic: String {
//        set {
//            self.subject = newValue
//        }
//        get {
//            return self.subject
//        }
//    }
    // 메서드 요구 준수
    func talk() {
        print("\(topic)에 대해 \(language)로 말합니다")
    }
    // 이니셜라이저 요구 준수
    init(topic: String, language: String) {
        self.topic = topic
        self.language = language
    }
}
```

> 프로퍼티 요구는 다양한 방법으로 해석, 구현할 수 있습니다.

```swift
struct Person: Talkable {
    var subject: String = ""
    // 프로퍼티 요구는 연산 프로퍼티로 대체가 가능합니다
    var topic: String {
        set {
            self.subject = newValue
        }
        get {
            return self.subject
        }
    }
    var language: String { return "한국어" }
    func talk() {
        print("\(topic)에 대해 \(language)로 말합니다")
    }
    init(topic: String, language: String) {
        self.topic = topic
    }
}
```

**프로토콜 상속**

프로토콜은 하나 이상의 프로토콜을 상속받아 기존 프로토콜의 요구사항보다 더 많은 요구사항을 추가할 수 있습니다. 프로토콜 상속 문법은 클래스의 상속 문법과 유사하지만, 프로토콜은 클래스와 다르게 다중상속이 가능합니다.

```swift
protocol 프로토콜 이름: 부모 프로토콜 이름 목록 {
 /* 정의부 */
 }
protocol Readable {
    func read()
}
protocol Writeable {
    func write()
}
protocol ReadSpeakable: Readable {
    func speak()
}
protocol ReadWriteSpeakable: Readable, Writeable {
    func speak()
}
struct SomeType: ReadWriteSpeakable {
    func read() {
        print("Read")
    }
    func write() {
        print("Write")
    }
    func speak() {
        print("Speak")
    }
}
```

**클래스 상속과 프로토콜**

클래스에서 상속과 프로토콜 채택을 동시에 하려면 상속받으려는 클래스를 먼저 명시하고 그 뒤에 채택할 프로토콜 목록을 작성합니다.

```swift
class SuperClass: Readable {
    func read() { }
}
class SubClass: SuperClass, Writeable, ReadSpeakable {
    func write() { }
    func speak() { }
}
```

**프로토콜 준수 확인**

is, as 연산자를 사용해서 인스턴스가 특정 프로토콜을 준수하는지 확인할 수 있습니다.

```swift
let sup: SuperClass = SuperClass()
let sub: SubClass = SubClass()
var someAny: Any = sup
someAny is Readable // true
someAny is ReadSpeakable // false
someAny = sub
someAny is Readable // true
someAny is ReadSpeakable // true
someAny = sup
if let someReadable: Readable = someAny as? Readable {
    someReadable.read()
} // read
if let someReadSpeakable: ReadSpeakable = someAny as? ReadSpeakable {
    someReadSpeakable.speak()
} // 동작하지 않음
someAny = sub
if let someReadable: Readable = someAny as? Readable {
    someReadable.read()
} // read
```

---
출처 : 야곰 닷넷 - 스위프트 기초 [프로토콜](https://yagom.net/courses/swift-basic/lessons/%ed%83%80%ec%9e%85-%ed%99%95%ec%9e%a5/topic/%ed%94%84%eb%a1%9c%ed%86%a0%ec%bd%9c/)
___

### 익스텐션

익스텐션(Extension) 은 스위프트의 **강력한 기능** 중 하나입니다. 익스텐션은 구조체, 클래스, 열거형, 프로토콜 타입에 새로운 **기능을 추가**할 수 있는 기능입니다.

기능을 추가하려는 타입의 구현된 소스 코드를 알지 못하거나 볼 수 없다 해도, 타입만 알고 있다면 그 타입의 기능을 확장할 수도 있습니다.

* 스위프트의 익스텐션이 타입에 추가할 수 있는 기능
    * 연산 타입 프로퍼티 / 연산 인스턴스 프로퍼티
    * 타입 메서드 / 인스턴스 메서드
    * 이니셜라이저
    * 서브스크립트
    * 중첩 타입
    * 특정 프로토콜을 준수할 수 있도록 기능 추가

익스텐션은 타입에 새로운 기능을 추가할 수는 있지만, 기존에 존재하는 기능을 **재정의할 수는 없습니다.**

클래스의 상속과 익스텐션을 비교해보겠습니다. 이 둘은 비슷해보이지만 실제 성격은 많이 다릅니다.

클래스의 상속은 클래스 타입에서만 가능하지만 익스텐션은 구조체, 클래스, 프로토콜 등에 적용이 가능합니다. 또 클래스의 상속은 특정 타입을 물려받아 하나의 새로운 타입을 정의하고 추가 기능을 구현하는 수직 확장이지만, 익스텐션은 기존의 타입에 기능을 추가하는 수평 확장입니다. 또, 상속을 받으면 기존 기능을 재정의할 수 있지만, 익스텐션은 재정의할 수 없다는 것도 큰 차이 중 하나입니다. 상황과 용도에 맞게 상속과 익스텐션을 선택하여 사용하면 됩니다.

|  | 상속 | 익스텐션 |
| :----: | :----: | :----: |
| 확장 | 수직 확장 |     수평 확장     |
| 사용 | 클래스 타입 | 클래스, 구조체, 프로토콜, 제네릭 등 모든 타입 |
| 재정의 | 가능 | 불가능 |

익스텐션을 사용하는 대신 원래 타입을 정의한 소스에 기능을 추가하는 방법도 있겠지만, 외부 라이브러리나 프레임워크를 가져다 썼다면 원본 소스를 수정하지 못합니다. 이처럼 외부에서 가져온 타입에 내가 원하는 기능을 추가하고자 할 때 익스텐션을 사용합니다. 따로 상속을 받지 않아도 되며, 구조체와 열거형에도 기능을 추가할 수 있으므로 익스텐션은 매우 편리한 기능입니다.
익스텐션은 모든 타입에 적용할 수 있습니다. 모든 타입이라 함은 구조체, 열거형, 클래스, 프로토콜, 제네릭 타입 등을 뜻합니다. 즉, 익스텐션을 통해 모든 타입에 연산 프로퍼티, 메서드, 이니셜라이저, 서브스크립트, 중첩 데이터 타입 등을 추가할 수 있습니다.
더불어 익스텐션은 **프로토콜과 함께 사용**하면 굉장히 강력한 기능을 선사합니다. 이 부분은 **프로토콜 중심 프로그래밍(Protocol Oriented Programming)**에 대해 더 알아보면 좋습니다.

**정의 문법**

extension 키워드를 사용하여 정의합니다.

```swift
extension 확장할 타입 이름 {
    /* 타입에 추가될 새로운 기능 구현 */
}
```

익스텐션은 기존에 존재하는 타입이 추가적으로 다른 프로토콜을 채택할 수 있도록 확장할 수도 있습니다. 이런 경우에는 클래스나 구조체에서 사용하던 것과 똑같은 방법으로 프로토콜 이름을 나열해줍니다.

```swift
extension 확장할 타입 이름: 프로토콜1, 프로토콜2, 프로토콜3... {
    /* 프로토콜 요구사항 구현 */
}
```

스위프트 라이브러리를 살펴보면 실제로 익스텐션이 굉장히 많이 사용되고 있음을 알 수 있습니다. Double 타입에는 수많은 프로퍼티와 메서드, 이니셜라이저가 정의되어 있으며 수많은 프로토콜을 채택하고 있을 것이라고 예상되지만, 실제로 Double 타입의 정의를 살펴보면 그 모든것이 다 정의되어 있지는 않습니다.
그러면 Double 타입이 채택하고 준수해야 하는 수많은 프로토콜은 어디로 갔을까요? 어디에서 채택하고 어디에서 준수하도록 정의되어 있을까요? 당연히 답은 익스텐션입니다. 이처럼 스위프트 표준 라이브러리 타입의 기능은 대부분 익스텐션으로 구현되어 있습니다. Double 외에도 다른 타입들의 정의와 익스텐션을 찾아보면 더 많은 예를 보실 수 있습니다. 꼭 한 번 찾아보세요!

**익스텐션 구현**

연산 프로퍼티 추가

```swift
extension Int {
    var isEven: Bool {
        return self % 2 == 0
    }
    var isOdd: Bool {
        return self % 2 == 1
    }
}
print(1.isEven) // false
print(2.isEven) // true
print(1.isOdd)  // true
print(2.isOdd)  // false
var number: Int = 3
print(number.isEven) // false
print(number.isOdd) // true
number = 2
print(number.isEven) // true
print(number.isOdd) // false
```

위 코드의 익스텐션은 Int 타입에 두 개의 **연산 프로퍼티**를 추가한 것입니다. Int 타입의 인스턴스가 홀수인지 짝수인지 판별하여 Bool 타입으로 알려주는 연산 프로퍼티입니다. 익스텐션으로 Int 타입에 추가해준 연산 프로퍼티는 Int 타입의 어떤 인스턴스에도 사용이 가능합니다. 위의 코드처럼 인스턴스 연산 프로퍼티를 추가할 수도 있으며, static 키워드를 사용하여 타입 연산 프로퍼티도 추가할 수 있습니다.

**메서드 추가**

```swift
extension Int {
    func multiply(by n: Int) -> Int {
        return self * n
    }
}
print(3.multiply(by: 2))  // 6
print(4.multiply(by: 5))  // 20
number = 3
print(number.multiply(by: 2))   // 6
print(number.multiply(by: 3))   // 9
```

위 코드의 익스텐션을 통해 Int 타입에 인스턴스 메서드인 multiply(by:) 메서드를 추가했습니다. 여러 기능을 여러 익스텐션 블록으로 나눠서 구현해도 전혀 문제가 없습니다. 관련된 기능별로 하나의 익스텐션 블록에 묶어주는 것도 좋습니다.

**이니셜라이저 추가**

```swift
extension String {
    init(int: Int) {
        self = "\(int)"
    }
    init(double: Double) {
        self = "\(double)"
    }
}
let stringFromInt: String = String(int: 100)
// "100"
let stringFromDouble: String = String(double: 100.0)
// "100.0"
```

인스턴스를 초기화(이니셜라이즈)할 때 인스턴스 초기화에 필요한 다양한 데이터를 전달받을 수 있도록 여러 종류의 이니셜라이저를 만들 수 있습니다. 타입의 정의부에 이니셜라이저를 추가하지 않더라도 익스텐션을 통해 이니셜라이저를 추가할 수 있습니다.
하지만 익스텐션으로 클래스 타입에 편의 이니셜라이저는 추가할 수 있지만, 지정 이니셜라이저는 추가할 수 없습니다. 지정 이니셜라이저와 디이니셜라이저는 반드시 클래스 타입의 구현부에 위치해야 합니다(값 타입은 상관없습니다).

---
출처 : 야곰 닷넷 - 스위프트 기초 [익스텐션](https://yagom.net/courses/swift-basic/lessons/%ed%83%80%ec%9e%85-%ed%99%95%ec%9e%a5/topic/%ec%9d%b5%ec%8a%a4%ed%85%90%ec%85%98/)
___

### 오류처리

스위프트에서 오류(Error)는 Error라는 프로토콜을 준수하는 타입의 값을 통해 표현됩니다. Error 프로토콜은 사실상 요구사항이 없는 빈 프로토콜일 뿐이지만, 오류를 표현하기 위한 타입(주로 열거형)은 이 프로토콜을 채택합니다.

스위프트의 열거형은 오류의 종류를 나타내기에 아주 적합한 기능입니다. 연관 값을 통해 오류에 관한 부가 정보를 제공할 수도 있습니다.

이번 예제에는 프로그램 내에서 자판기를 작동시키려고 할 때 발생하는 오류상황을 구현해 보았습니다.

**오류표현**

Error 프로토콜과 (주로)열거형을 통해서 오류를 표현합니다

```swift
enum 오류종류이름: Error {
    case 종류1
    case 종류2
    case 종류3
    //...
}
```

> 자판기 동작 오류의 종류를 표현한 VendingMachineError 열거형

```swift
enum VendingMachineError: Error {
    case invalidInput
    case insufficientFunds(moneyNeeded: Int)
    case outOfStock
}
```

**함수에서 발생한 오류 던지기**

자판기 동작 도중 발생한 오류를 던지는 메서드를 구현해봅니다.
오류 발생의 여지가 있는 메서드는 throws를 사용하여 오류를 내포하는 함수임을 표시합니다.

```swift
class VendingMachine {
    let itemPrice: Int = 100
    var itemCount: Int = 5
    var deposited: Int = 0
    // 돈 받기 메서드
    func receiveMoney(_ money: Int) throws {
        // 입력한 돈이 0이하면 오류를 던집니다
        guard money > 0 else {
            throw VendingMachineError.invalidInput
        }
        // 오류가 없으면 정상처리를 합니다
        self.deposited += money
        print("\(money)원 받음")
    }
    // 물건 팔기 메서드
    func vend(numberOfItems numberOfItemsToVend: Int) throws -> String {
        // 원하는 아이템의 수량이 잘못 입력되었으면 오류를 던집니다
        guard numberOfItemsToVend > 0 else {
            throw VendingMachineError.invalidInput
        }
        // 구매하려는 수량보다 미리 넣어둔 돈이 적으면 오류를 던집니다
        guard numberOfItemsToVend * itemPrice <= deposited else {
            let moneyNeeded: Int
            moneyNeeded = numberOfItemsToVend * itemPrice - deposited
            throw VendingMachineError.insufficientFunds(moneyNeeded: moneyNeeded)
        }
        // 구매하려는 수량보다 요구하는 수량이 많으면 오류를 던집니다
        guard itemCount >= numberOfItemsToVend else {
            throw VendingMachineError.outOfStock
        }
        // 오류가 없으면 정상처리를 합니다
        let totalPrice = numberOfItemsToVend * itemPrice
        self.deposited -= totalPrice
        self.itemCount -= numberOfItemsToVend
        return "\(numberOfItemsToVend)개 제공함"
    }
}
// 자판기 인스턴스
let machine: VendingMachine = VendingMachine()
// 판매 결과를 전달받을 변수
var result: String?
```

**오류처리**

오류를 던질 수도 있지만 오류가 던져지는 것에 대비하여 던져진 오류를 처리하기 위한 코드도 작성해야 합니다. 예를 들어 던져진 오류가 무엇인지 판단하여 다시 문제를 해결한다든지, 다른 방법으로 시도해 본다든지, 사용자에게 오류를 알리고 사용자에게 선택 권한을 넘겨주어 다음에 어떤 동작을 하게 할 것인지 결정하도록 유도하는 등의 코드를 작성해야 합니다.

오류발생의 여지가 있는 throws 함수(메서드)는 try를 사용하여 호출해야합니다.
try와 do-catch, try?와 try! 등에 대해 알아봅니다.

do-catch
오류발생의 여지가 있는 throws 함수(메서드)는 do-catch 구문을 활용하여 오류발생에 대비합니다.

> 가장 정석적인 방법으로 모든 오류 케이스에 대응합니다

```swift
do {
    try machine.receiveMoney(0)
} catch VendingMachineError.invalidInput {
    print("입력이 잘못되었습니다")
} catch VendingMachineError.insufficientFunds(let moneyNeeded) {
    print("\(moneyNeeded)원이 부족합니다")
} catch VendingMachineError.outOfStock {
    print("수량이 부족합니다")
} // 입력이 잘못되었습니다
```

> 하나의 catch 블럭에서 switch 구문을 사용하여 오류를 분류해봅니다. 굳이 위의 것과 크게 다를 것이 없습니다.

```swift
do {
    try machine.receiveMoney(300)
} catch /*(let error)*/ {
    switch error {
    case VendingMachineError.invalidInput:
        print("입력이 잘못되었습니다")
    case VendingMachineError.insufficientFunds(let moneyNeeded):
        print("\(moneyNeeded)원이 부족합니다")
    case VendingMachineError.outOfStock:
        print("수량이 부족합니다")
    default:
        print("알수없는 오류 \(error)")
    }
} // 300원 받음
```

> 딱히 케이스별로 오류처리 할 필요가 없으면 catch 구문 내부를 간략화해도 무방합니다.

```swift
do {
    result = try machine.vend(numberOfItems: 4)
} catch {
    print(error)
} // insufficientFunds(100)

> 딱히 케이스별로 오류처리 할 필요가 없으면 do 구문만 써도 무방합니다

do {
    result = try machine.vend(numberOfItems: 4)
}
```

**try? 와 try!**

**try?**

별도의 오류처리 결과를 통보받지 않고 오류가 발생했으면 결과값을 nil로 돌려받을 수 있습니다. 정상동작 후에는 옵셔널 타입으로 정상 반환값을 돌려 받습니다.

```swift
result = try? machine.vend(numberOfItems: 2)
result // Optional("2개 제공함")
result = try? machine.vend(numberOfItems: 2)
result // nil
```

**try!**

오류가 발생하지 않을 것이라는 강력한 확신을 가질 때 try!를 사용하면 정상동작 후에 바로 결과값을 돌려받습니다. 오류가 발생하면 런타임 오류가 발생하여 애플리케이션 동작이 중지됩니다.

```swift
result = try! machine.vend(numberOfItems: 1)
result // 1개 제공함
//result = try! machine.vend(numberOfItems: 1)
// 런타임 오류 발생!
```

---
출처 : 야곰 닷넷 - 스위프트 기초 [오류처리](https://yagom.net/courses/swift-basic/lessons/%ec%98%a4%eb%a5%98%ec%b2%98%eb%a6%ac/)
___

###### tags: `swift`
