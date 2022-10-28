# 2022-10-28 Swift

## 사용자정의 타입

### 구조체

**정의 문법**

스위프트 대부분의 타입은 구조체로 이루어져 있습니다. 구조체는 값 타입입니다. 타입이름은 대문자 카멜케이스를 사용하여 정의합니다.

```swift
struct 이름 {
    /* 구현부 */
}
```

**프로퍼티 및 메서드 구현**

```swift
struct Sample {
    // 가변 프로퍼티
    var mutableProperty: Int = 100 

    // 불변 프로퍼티
    let immutableProperty: Int = 100 

    // 타입 프로퍼티
    static var typeProperty: Int = 100 

    // 인스턴스 메서드
    func instanceMethod() {
        print("instance method")
    }

    // 타입 메서드
    static func typeMethod() {
        print("type method")
    }
}
```

**구조체 사용**

```swift
// 가변 인스턴스 생성
var mutable: Sample = Sample()

mutable.mutableProperty = 200

// 불변 프로퍼티는 인스턴스 생성 후 수정할 수 없습니다
// 컴파일 오류 발생
//mutable.immutableProperty = 200

// 불변 인스턴스
let immutable: Sample = Sample()

// 불변 인스턴스는 아무리 가변 프로퍼티라도
// 인스턴스 생성 후에 수정할 수 없습니다
// 컴파일 오류 발생
//immutable.mutableProperty = 200
//immutable.immutableProperty = 200

// 타입 프로퍼티 및 메서드
Sample.typeProperty = 300
Sample.typeMethod() // type method

// 인스턴스에서는 타입 프로퍼티나 타입 메서드를
// 사용할 수 없습니다
// 컴파일 오류 발생
//mutable.typeProperty = 400
//mutable.typeMethod()
```

**학생 구조체 만들어보기**

```swift
struct Student {
    // 가변 프로퍼티
    var name: String = "unknown"

    // 키워드도 `로 묶어주면 이름으로 사용할 수 있습니다
    var `class`: String = "Swift"

    // 타입 메서드
    static func selfIntroduce() {
        print("학생타입입니다")
    }

    // 인스턴스 메서드
    // self는 인스턴스 자신을 지칭하며, 몇몇 경우를 제외하고 사용은 선택사항입니다
    func selfIntroduce() {
        print("저는 \(self.class)반 \(name)입니다")
    }
}

// 타입 메서드 사용
Student.selfIntroduce() // 학생타입입니다

// 가변 인스턴스 생성
var yagom: Student = Student()
yagom.name = "yagom"
yagom.class = "스위프트"
yagom.selfIntroduce()   // 저는 스위프트반 yagom입니다

// 불변 인스턴스 생성
let jina: Student = Student()

// 불변 인스턴스이므로 프로퍼티 값 변경 불가
// 컴파일 오류 발생
//jina.name = "jina"
jina.selfIntroduce() // 저는 Swift반 unknown입니다
```

---
출처 : 야곰 닷넷 - 스위프트 기초 [구조체](https://yagom.net/courses/swift-basic/lessons/%ec%82%ac%ec%9a%a9%ec%9e%90%ec%a0%95%ec%9d%98-%ed%83%80%ec%9e%85/topic/%ea%b5%ac%ec%a1%b0%ec%b2%b4/)
___

### 클래스

**정의 문법**

클래스는 참조 타입입니다. 타입이름은 대문자 카멜케이스를 사용하여 정의합니다.

```swift
class 이름 {
    /* 구현부 */
}
```

**프로퍼티 및 메서드 구현**

클래스의 타입 메서드는 두 종류가 있습니다. 상속 후 재정의가 가능한 class타입메서드, 상속 후 재정의가 불가능한 static 타입메서드가 있습니다. 자세한 내용은 상속 부분에서 다시 다룹니다.

```swift
class Sample {
    // 가변 프로퍼티
    var mutableProperty: Int = 100 

    // 불변 프로퍼티
    let immutableProperty: Int = 100 

    // 타입 프로퍼티
    static var typeProperty: Int = 100 

    // 인스턴스 메서드
    func instanceMethod() {
        print("instance method")
    }

    // 타입 메서드
    // 재정의 불가 타입 메서드 - static
    static func typeMethod() {
        print("type method - static")
    }

    // 재정의 가능 타입 메서드 - class
    class func classMethod() {
        print("type method - class")
    }
}
```

**클래스 사용**

```swift
// 인스턴스 생성 - 참조정보 수정 가능
var mutableReference: Sample = Sample()

mutableReference.mutableProperty = 200

// 불변 프로퍼티는 인스턴스 생성 후 수정할 수 없습니다
// 컴파일 오류 발생
//mutableReference.immutableProperty = 200

// 인스턴스 생성 - 참조정보 수정 불가
let immutableReference: Sample = Sample()

// 클래스의 인스턴스는 참조 타입이므로 let으로 선언되었더라도 인스턴스 프로퍼티의 값 변경이 가능합니다
immutableReference.mutableProperty = 200

// 다만 참조정보를 변경할 수는 없습니다
// 컴파일 오류 발생
//immutableReference = mutableReference

// 참조 타입이라도 불변 인스턴스는 
// 인스턴스 생성 후에 수정할 수 없습니다
// 컴파일 오류 발생
//immutableReference.immutableProperty = 200

// 타입 프로퍼티 및 메서드
Sample.typeProperty = 300
Sample.typeMethod() // type method

// 인스턴스에서는 타입 프로퍼티나 타입 메서드를
// 사용할 수 없습니다
// 컴파일 오류 발생
//mutableReference.typeProperty = 400
//mutableReference.typeMethod()
```

**학생 클래스 만들어보기**

```swift
class Student {
    // 가변 프로퍼티
    var name: String = "unknown"

    // 키워드도 `로 묶어주면 이름으로 사용할 수 있습니다
    var `class`: String = "Swift"

    // 타입 메서드
    class func selfIntroduce() {
        print("학생타입입니다")
    }

    // 인스턴스 메서드
    // self는 인스턴스 자신을 지칭하며, 몇몇 경우를 제외하고 사용은 선택사항입니다
    func selfIntroduce() {
        print("저는 \(self.class)반 \(name)입니다")
    }
}

// 타입 메서드 사용
Student.selfIntroduce() // 학생타입입니다

// 인스턴스 생성
var yagom: Student = Student()
yagom.name = "yagom"
yagom.class = "스위프트"
yagom.selfIntroduce()   // 저는 스위프트반 yagom입니다

// 인스턴스 생성
let jina: Student = Student()
jina.name = "jina"
jina.selfIntroduce() // 저는 Swift반 jina입니다
```

---
출처 : 야곰 닷넷 - 스위프트 기초 [클래스](https://yagom.net/courses/swift-basic/lessons/%ec%82%ac%ec%9a%a9%ec%9e%90%ec%a0%95%ec%9d%98-%ed%83%80%ec%9e%85/topic/%ed%81%b4%eb%9e%98%ec%8a%a4/)
___

### 열거형

**정의 문법**

스위프트의 열거형은 다른 언어의 열거형과는 많이 다릅니다. 잘 살펴보아야 할 스위프트의 기능 중 하나입니다.

* enum은 타입이므로 대문자 카멜케이스를 사용하여 이름을 정의합니다
* 각 case는 소문자 카멜케이스로 정의합니다
* 각 case는 그 자체가 고유의 값입니다
* 각 케이스는 한 줄에 개별로도, 한 줄에 여러개도 정의할 수 있습니다

```swift
enum 이름 {
    case 이름1
    case 이름2
    case 이름3, 이름4, 이름5
    // ...
}
```

**열거형 사용**

```swift
enum Weekday {
    case mon
    case tue
    case wed
    case thu, fri, sat, sun
}

// 열거형 타입과 케이스를 모두 사용하여도 됩니다
var day: Weekday = Weekday.mon

// 타입이 명확하다면 .케이스 처럼 표현해도 무방합니다
day = .tue

print(day) // tue

// switch의 비교값에 열거형 타입이 위치할 때
// 모든 열거형 케이스를 포함한다면
// default를 작성할 필요가 없습니다
switch day {
case .mon, .tue, .wed, .thu:
    print("평일입니다")
case Weekday.fri:
    print("불금 파티!!")
case .sat, .sun:
    print("신나는 주말!!")
}
// 평일입니다
```

**원시값**

c 언어의 enum처럼 정수값을 가질 수도 있습니다. rawValue를 사용하면 됩니다.
*case별로 각각 다른값을 가져야합니다*

```swift
enum Fruit: Int {
    case apple = 0
    case grape = 1
    case peach

    // mango와 apple의 원시값이 같으므로 
    // mango 케이스의 원시값을 0으로 정의할 수 없습니다
//    case mango = 0
}

print("Fruit.peach.rawValue == \(Fruit.peach.rawValue)")
// Fruit.peach.rawValue == 2
```

정수 타입 뿐만 아니라 *Hashable* 프로토콜을 따르는 모든 타입이 원시값의 타입으로 지정될 수 있습니다.

```swift
enum School: String {
    case elementary = "초등"
    case middle = "중등"
    case high = "고등"
    case university
}

print("School.middle.rawValue == \(School.middle.rawValue)")
// School.middle.rawValue == 중등

// 열거형의 원시값 타입이 String일 때, 원시값이 지정되지 않았다면
// case의 이름을 원시값으로 사용합니다
print("School.university.rawValue == \(School.university.rawValue)")
// School.university.rawValue == university
```

**원시값을 통한 초기화**

rawValue를 통해 초기화 할 수 있습니다. rawValue가 case에 해당하지 않을 수 있으므로 rawValue를 통해 초기화 한 인스턴스는 옵셔널 타입입니다.

```swift
// rawValue를 통해 초기화 한 열거형 값은 옵셔널 타입이므로 Fruit 타입이 아닙니다
//let apple: Fruit = Fruit(rawValue: 0)
let apple: Fruit? = Fruit(rawValue: 0)

// if let 구문을 사용하면 rawValue에 해당하는 케이스를 곧바로 사용할 수 있습니다
if let orange: Fruit = Fruit(rawValue: 5) {
    print("rawValue 5에 해당하는 케이스는 \(orange)입니다")
} else {
    print("rawValue 5에 해당하는 케이스가 없습니다")
} // rawValue 5에 해당하는 케이스가 없습니다
```

**메서드**

스위프트의 열거형에는 메서드도 추가할 수 있습니다

```swift
enum Month {
    case dec, jan, feb
    case mar, apr, may
    case jun, jul, aug
    case sep, oct, nov

    func printMessage() {
        switch self {
        case .mar, .apr, .may:
            print("따스한 봄~")
        case .jun, .jul, .aug:
            print("여름 더워요~")
        case .sep, .oct, .nov:
            print("가을은 독서의 계절!")
        case .dec, .jan, .feb:
            print("추운 겨울입니다")
        }
    }
}

Month.mar.printMessage()
// 따스한 봄~
```

---
출처 : 야곰 닷넷 - 스위프트 기초 [열거형](https://yagom.net/courses/swift-basic/lessons/%ec%82%ac%ec%9a%a9%ec%9e%90%ec%a0%95%ec%9d%98-%ed%83%80%ec%9e%85/topic/%ec%97%b4%ea%b1%b0%ed%98%95/)
___

### 값 타입과 참조 타입

**Class**

* 전통적인 OOP 관점에서의 클래스
* 단일상속
* (인스턴스/타입) 메서드
* (인스턴스/타입) 프로퍼티
* **참조 타입**
* Apple 프레임워크의 대부분의 큰 뼈대는 모두 클래스로 구성

**Struct**

* C언어 등의 구조체보다 다양한 기능
* 상속 불가
* (인스턴스/타입) 메서드
* (인스턴스/타입) 프로퍼티
* **값 타입**
* Swift의 대부분의 큰 뼈대는 모두 구조체로 구성

**Enum**

* 다른 언어의 열거형과는 많이 다른 존재
* 상속 불가
* (인스턴스/타입) 메서드
* (인스턴스/타입) 연산 프로퍼티
* **값 타입**
* Enumeration
* 유사한 종류의 여러 값을 유의미한 이름으로 한 곳에 모아 정의
    * 예) 요일, 상태값, 월(Month) 등
* **열거형 자체가 하나의 데이터 타입**
    * **열거형 case하나하나 전부 하나의 유의미한 값으로 취급**
* 선언 키워드 - enum

**Class / Struct / Enum**

|  | **Class** | **Struct** | **Enum** |
| :----: | :-----: | :--------: | :---: |
| **Type** | Reference | Value | Value |
| **Subclassing** | O | X | X |
| **Extension** | O | O | O |

**구조체는 언제 사용하나?**

* 연관된 몇몇의 값들을 모아서 하나의 데이터타입으로 표현하고 싶을 때
* 다른 객체 또는 함수 등으로 전달될 때 **참조가 아닌 복사를 원할 때**
* 자신을 상속할 필요가 없거나, 자신이 다른 타입을 **상속받을 필요가 없을 때**
* Apple 프레임워크에서 프로그래밍을 할 때에는 주로 클래스를 많이 사용

**Value vs Reference**

* Value
    * 데이터를 전달할 때 값을 복사하여 전달

* Reference
    * 데이터를 전달할 때 값의 메모리 위치를 전달

열거형과 구조체는 값 타입이며, 클래스는 참조 타입이라는 것이 가장 큰 차이입니다. 또한, 클래스는 상속이 가능하지만 구조체와 열거형은 상속이 불가능합니다

```swift
struct ValueType {
    var property = 1
}

class ReferenceType {
    var property = 1
}

// 첫 번째 구조체 인스턴스
let firstStructInstance = ValueType()
// 두 번째 구조체 인스턴스에 첫 번째 인스턴스 값 복사
var secondStructInstance = firstStructInstance
// 두 번째 구조체 인스턴스 프로퍼티 값 수정
secondStructInstance.property = 2

// 두 번째 구조체 인스턴스는 첫 번째 구조체를 똑같이 복사한 
// 별도의 인스턴스이기 때문에 
// 두 번째 구조체 인스턴스의 프로퍼티 값을 변경해도
// 첫 번째 구조체 인스턴스의 프로퍼티 값에는 영향이 없음
print("first struct instance property : \(firstStructInstance.property)")    // 1
print("second struct instance property : \(secondStructInstance.property)")  // 2

// 클래스 인스턴스 생성 후 첫 번째 참조 생성
let firstClassReference = ReferenceType()
// 두 번째 참조 변수에 첫 번째 참조 할당
let secondClassReference = firstClassReference
secondClassReference.property = 2

// 두 번째 클래스 참조는 첫 번째 클래스 인스턴스를 참조하기 때문에
// 두 번째 참조를 통해 인스턴스의 프로퍼티 값을 변경하면
// 첫 번째 클래스 인스턴스의 프로퍼티 값을 변경하게 됨
print("first class reference property : \(firstClassReference.property)")    // 2
print("second class reference property : \(secondClassReference.property)")  // 2
```

**Data types in Swift**

* public struct **Int**
* public struct **Double**
* public struct **String**
* public struct **Dictioary<Key : Hashable, Value>**
* public struct **Array\<Element>**
* public struct **Set\<Element : hashable>**

**Swift LOVEs Struct**

* 스위프트는 구조체, 열거형 사용을 선호
* Apple 프레임워크는 대부분 클래스 사용
* Apple 프레임워크 사용시 구조체/클래스 선택은 우리의 몫

---
출처 : 야곰 닷넷 - 스위프트 기초 [값 타입과 참조 타입](https://yagom.net/courses/swift-basic/lessons/%ec%82%ac%ec%9a%a9%ec%9e%90%ec%a0%95%ec%9d%98-%ed%83%80%ec%9e%85/topic/%ea%b0%92-%ed%83%80%ec%9e%85%ea%b3%bc-%ec%b0%b8%ec%a1%b0-%ed%83%80%ec%9e%85/)
___

## 클로저

### 클로저 기본

클로저는 코드의 블럭입니다. [일급시민(first-citizen)](https://ko.wikipedia.org/wiki/%EC%9D%BC%EA%B8%89_%EA%B0%9D%EC%B2%B4)으로, 전달인자, 변수, 상수 등으로 저장, 전달이 가능합니다. 함수는 클로저의 일종으로, *이름이 있는 클로저*라고 생각하면 됩니다.

**기본 클로저 문법**

```swift
{ (매개변수 목록) -> 반환타입 in
    실행 코드
}
```

**클로저의 사용**

```swift
// sum이라는 상수에 클로저를 할당
let sum: (Int, Int) -> Int = { (a: Int, b: Int) in
    return a + b
}
let sumResult: Int = sum(1, 2)
print(sumResult) // 3
```

**함수의 전달인자로서의 클로저**

클로저는 주로 함수의 전달인자로 많이 사용됩니다. 함수 내부에서 원하는 코드블럭을 실행할 수 있습니다.

```swift
let add: (Int, Int) -> Int
add = { (a: Int, b: Int) in
    return a + b
}
let substract: (Int, Int) -> Int
substract = { (a: Int, b: Int) in
    return a - b
}
let divide: (Int, Int) -> Int
divide = { (a: Int, b: Int) in
    return a / b
}
func calculate(a: Int, b: Int, method: (Int, Int) -> Int) -> Int {
    return method(a, b)
}
var calculated: Int
calculated = calculate(a: 50, b: 10, method: add)
print(calculated) // 60
calculated = calculate(a: 50, b: 10, method: substract)
print(calculated) // 40
calculated = calculate(a: 50, b: 10, method: divide)
print(calculated) // 5
//따로 클로저를 상수/변수에 넣어 전달하지 않고,
//함수를 호출할 때 클로저를 작성하여 전달할 수도 있습니다.
calculated = calculate(a: 50, b: 10, method: { (left: Int, right: Int) -> Int in
    return left * right
})
print(calculated) // 500
```

### 다양한 클로저표현

클로저는 다양한 모습으로 표현될 수 있습니다.

> 함수의 매개변수 마지막으로 전돨되는 클로저는 *후행클로저(trailing closure)* 로 함수 밖에 구현할 수 있습니다.

> 컴파일러가 클로저의 타입을 유추할 수 있는 경우 매개변수, 반환 타입을 생략할 수 있습니다.

> 반환 값이 있는경우, 암시적으로 클로저의 맨 마지막 줄은 return 키워드를 생략하더라도 반환 값으로 취급합니다.

> 전달인자의 이름이 굳이 필요없고, 컴파일러가 타입을 유추할 수 있는 경우 축약된 전달인자 이름(*\$0, \$1, \$2...*\)을 사용할 수 있습니다.

클로저 매개변수를 갖는 함수 *calculate(a\: b\: method\:)* 와 결과값을 저장할 변수 *result* 를 먼저 선언해둡니다.

```swift
func calculate(a: Int, b: Int, method: (Int, Int) -> Int) -> Int {
    return method(a, b)
}

var result: Int
```

**후행 클로저**

클로저가 함수의 마지막 전달인자라면 매개변수 이름을 생략한 후 함수 소괄호 외부에 클로저를 구현할 수 있습니다.

```swift
result = calculate(a: 10, b: 10) { (left: Int, right: Int) -> Int in
    return left + right
}

print(result) // 20
```

**반환타입 생략**

*calculate(a\: b\: method\:)* 함수의 *method* 매개변수는 *Int* 타입을 반환할 것이라는 사실을 컴파일러도 알기 때문에 굳이 클로저에서 반환타입을 명시해 주지 않아도 됩니다.
대신 *in 키워드는 생략할 수 없습니다*

```swift
result = calculate(a: 10, b: 10, method: { (left: Int, right: Int) in
    return left + right
})

print(result) // 20

// 후행클로저와 함께 사용할 수도 있습니다
result = calculate(a: 10, b: 10) { (left: Int, right: Int) in
    return left + right
}

print(result) // 20
```

**단축 인자이름**

클로저의 매개변수 이름이 굳이 불필요하다면 단축 인자이름을 활용할 수 있습니다. 단축 인자이름은 클로저의 매개변수의 순서대로 *\$0, \$1, \$2…* 처럼 표현합니다.

```swift
result = calculate(a: 10, b: 10, method: {
    return $0 + $1
})

print(result) // 20

// 당연히 후행 클로저와 함께 사용할 수 있습니다
result = calculate(a: 10, b: 10) {
    return $0 + $1
}

print(result) // 20
```

**암시적 반환 표현**

클로저가 반환하는 값이 있다면 클로저의 마지막 줄의 결과값은 암시적으로 반환값으로 취급합니다.

```swift
result = calculate(a: 10, b: 10) {
    $0 + $1
}

print(result) // 20

// 간결하게 한 줄로 표현해 줄 수도 있습니다
result = calculate(a: 10, b: 10) { $0 + $1 }

print(result) // 20
```

**축약 전과 후 비교**

```swift
//축약 전
result = calculate(a: 10, b: 10, method: { (left: Int, right: Int) -> Int in
    return left + right
})

//축약 후
result = calculate(a: 10, b: 10) { $0 + $1 }

print(result) // 20
```

---
출처 : 야곰 닷넷 - 스위프트 기초 [클로저 기본](https://yagom.net/courses/swift-basic/lessons/%ed%81%b4%eb%a1%9c%ec%a0%80/topic/%ed%81%b4%eb%a1%9c%ec%a0%80-%ea%b8%b0%eb%b3%b8/)
___

###### tags: `swift`
