# 2022-11-07 Swift

## 타입 심화

### 프로퍼티

**프로퍼티의 종류**

* 인스턴스 저장 프로퍼티
* 타입 저장 프로퍼티
* 인스턴스 연산 프로퍼티
* 타입 연산 프로퍼티
* 지연 저장 프로퍼티

**정의와 사용**

프로퍼티는 구조체, 클래스, 열거형 내부에 구현할 수 있습니다. 다만 열거형 내부에는 연산 프로퍼티만 구현할 수 있습니다. 연산 프로퍼티는 var로만 선언할 수 있습니다.
연산프로퍼티를 읽기전용으로는 구현할 수 있지만, 쓰기 전용으로는 구현할 수 없습니다. 읽기전용으로 구현하려면 get 블럭만 작성해주면 됩니다. 읽기전용은 get블럭을 생략할 수 있습니다. 읽기, 쓰기 모두 가능하게 하려면 get 블럭과 set 블럭을 모두 구현해주면 됩니다. set 블럭에서 암시적 매개변수 newValue를 사용할 수 있습니다.

```swift
struct Student {
    // 인스턴스 저장 프로퍼티
    var name: String = ""
    var `class`: String = "Swift"
    var koreanAge: Int = 0
    // 인스턴스 연산 프로퍼티
    var westernAge: Int {
        get {
            return koreanAge - 1
        }
        set(inputValue) {
            koreanAge = inputValue + 1
        }
    }
    // 타입 저장 프로퍼티
    static var typeDescription: String = "학생"
    /*
    // 인스턴스 메서드
    func selfIntroduce() {
        print("저는 \(self.class)반 \(name)입니다")
    }
     */
    // 읽기전용 인스턴스 연산 프로퍼티
    // 간단히 위의 selfIntroduce() 메서드를 대체할 수 있습니다
    var selfIntroduction: String {
        get {
            return "저는 \(self.class)반 \(name)입니다"
        }
    }
    /*
     // 타입 메서드
     static func selfIntroduce() {
     print("학생타입입니다")
     }
     */
    // 읽기전용 타입 연산 프로퍼티
    // 읽기전용에서는 get을 생략할 수 있습니다
    static var selfIntroduction: String {
        return "학생타입입니다"
    }
}
// 타입 연산 프로퍼티 사용
print(Student.selfIntroduction)
// 학생타입입니다
// 인스턴스 생성
var yagom: Student = Student()
yagom.koreanAge = 10
// 인스턴스 저장 프로퍼티 사용
yagom.name = "yagom"
print(yagom.name)
// yagom
// 인스턴스 연산 프로퍼티 사용
print(yagom.selfIntroduction)
// 저는 Swift반 yagom입니다
print("제 한국나이는 \(yagom.koreanAge)살이고, 미쿡나이는 \(yagom.westernAge)살입니다.")
// 제 한국나이는 10살이고, 미쿡나이는 9살입니다.
```

**응용**

```swift
struct Money {
    var currencyRate: Double = 1100
    var dollar: Double = 0
    var won: Double {
        get {
            return dollar * currencyRate
        }
        set {
            dollar = newValue / currencyRate
        }
    }
}
var moneyInMyPocket = Money()
moneyInMyPocket.won = 11000
print(moneyInMyPocket.won)
// 11000.0
moneyInMyPocket.dollar = 10
print(moneyInMyPocket.won)
// 11000.0
```

**지역변수 및 전역변수**

저장 프로퍼티와 연산 프로퍼티의 기능은 함수, 메서드, 클로저, 타입 등의 외부에 위치한 지역/전역 변수에도 모두 사용 가능합니다.

```swift
var a: Int = 100
var b: Int = 200
var sum: Int {
    return a + b
}
print(sum) // 300
```

---
출처 : 야곰 닷넷 - 스위프트 기초 [프로퍼티](https://yagom.net/courses/swift-basic/lessons/%ed%83%80%ec%9e%85-%ec%8b%ac%ed%99%94/topic/%ed%94%84%eb%a1%9c%ed%8d%bc%ed%8b%b0/)
___

### 프로퍼티 감시자

프로퍼티 감시자를 사용하면 프로퍼티 값이 변경될 때 원하는 동작을 수행할 수 있습니다. 값이 변경되기 직전에 willSet블럭이, 값이 변경된 직후에 didSet블럭이 호출됩니다. 둘 중 필요한 하나만 구현해 주어도 무관합니다. 변경되려는 값이 **기존 값과 똑같더라도 프로퍼티 감시자**는 항상 동작합니다.
willSet 블럭에서 암시적 매개변수 newValue를 사용할 수 있고, didSet 블럭에서 암시적 매개변수 oldValue를 사용할 수 있습니다.

프로퍼티 감시자는 연산 프로퍼티에 사용할 수 없습니다.

**정의 및 사용**

```swift
struct Money {
    // 프로퍼티 감시자 사용
    var currencyRate: Double = 1100 {
        willSet(newRate) {
            print("환율이 \(currencyRate)에서 \(newRate)으로 변경될 예정입니다")
        }

        didSet(oldRate) {
            print("환율이 \(oldRate)에서 \(currencyRate)으로 변경되었습니다")
        }
    }

    // 프로퍼티 감시자 사용
    var dollar: Double = 0 {
        // willSet의 암시적 매개변수 이름 newValue
        willSet {
            print("\(dollar)달러에서 \(newValue)달러로 변경될 예정입니다")
        }

        // didSet의 암시적 매개변수 이름 oldValue
        didSet {
            print("\(oldValue)달러에서 \(dollar)달러로 변경되었습니다")
        }
    }

    // 연산 프로퍼티
    var won: Double {
        get {
            return dollar * currencyRate
        }
        set {
            dollar = newValue / currencyRate
        }

        /* 프로퍼티 감시자와 연산 프로퍼티 기능을 동시에 사용할 수 없습니다
        willSet {

        }
         */
    }    
}

var moneyInMyPocket: Money = Money()

// 환율이 1100.0에서 1150.0으로 변경될 예정입니다
moneyInMyPocket.currencyRate = 1150
// 환율이 1100.0에서 1150.0으로 변경되었습니다

// 0.0달러에서 10.0달러로 변경될 예정입니다
moneyInMyPocket.dollar = 10
// 0.0달러에서 10.0달러로 변경되었습니다

print(moneyInMyPocket.won)
// 11500.0
```

---
출처 : 야곰 닷넷 - 스위프트 기초 [프로퍼티 감시자](https://yagom.net/courses/swift-basic/lessons/%ed%83%80%ec%9e%85-%ec%8b%ac%ed%99%94/topic/%ed%94%84%eb%a1%9c%ed%8d%bc%ed%8b%b0-%ea%b0%90%ec%8b%9c%ec%9e%90/)
___

### 상속

스위프트의 상속은 클래스, 프로토콜 등에서 가능합니다. 열거형, 구조체는 상속이 불가능합니다. 스위프트는 다중상속을 지원하지 않습니다.

**클래스의 상속과 재정의**

상속 문법
```swift
class 이름: 상속받을 클래스 이름 {
    /* 구현부 */
}
```

```swift
// 기반 클래스 Person
class Person {
    var name: String = ""
    func selfIntroduce() {
        print("저는 \(name)입니다")
    }
    // final 키워드를 사용하여 재정의를 방지할 수 있습니다
    final func sayHello() {
        print("hello")
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
    // 재정의 가능한 class 메서드라도
    // final 키워드를 사용하면 재정의 할 수 없습니다
    // 메서드 앞의 `static`과 `final class`는 똑같은 역할을 합니다
    final class func finalCalssMethod() {
        print("type method - final class")
    }
}
// Person을 상속받는 Student
class Student: Person {
    var major: String = ""
    override func selfIntroduce() {
        print("저는 \(name)이고, 전공은 \(major)입니다")
    }
    override class func classMethod() {
        print("overriden type method - class")
    }
    // static을 사용한 타입 메서드는 재정의 할 수 없습니다
//    override static func typeMethod() {    }
    // final 키워드를 사용한 메서드, 프로퍼티는 재정의 할 수 없습니다
//    override func sayHello() {    }
//    override class func finalClassMethod() {    }
}
```

**동작 확인**

```swift
let yagom: Person = Person()
let hana: Student = Student()
yagom.name = "yagom"
hana.name = "hana"
hana.major = "Swift"
yagom.selfIntroduce()
// 저는 yagom입니다
hana.selfIntroduce()
// 저는 hana이고, 전공은 Swift입니다
Person.classMethod()
// type method - class
Person.typeMethod()
// type method - static
Person.finalCalssMethod()
// type method - final class
Student.classMethod()
// overriden type method - class
Student.typeMethod()
// type method - static
Student.finalCalssMethod()
// type method - final class
```

---
출처 : 야곰 닷넷 - 스위프트 기초 [상속](https://yagom.net/courses/swift-basic/lessons/%ed%83%80%ec%9e%85-%ec%8b%ac%ed%99%94/topic/%ec%83%81%ec%86%8d/)
___

### 인스턴스의 생성과 소멸

**프로퍼티 기본값**

스위프트의 모든 인스턴스는 초기화와 동시에 모든 프로퍼티에 유효한 값이 할당되어 있어야 합니다. 프로퍼티에 미리 기본값을 할당해두면 인스턴스가 생성됨과 동시에 초기값을 지니게 됩니다.

```swift
class PersonA {
    // 모든 저장 프로퍼티에 기본값 할당
    var name: String = "unknown"
    var age: Int = 0
    var nickName: String = "nick"
}
// 인스턴스 생성
let jason: PersonA = PersonA()
// 기본값이 인스턴스가 지녀야 할 값과 맞지 않다면
// 생성된 인스턴스의 프로퍼티에 각각 값 할당
jason.name = "jason"
jason.age = 30
jason.nickName = "j"
```

**이니셜라이저**

프로퍼티 기본값을 지정하기 어려운 경우에는 이니셜라이저 init을 통해 인스턴스가 가져야 할 초기값을 전달할 수 있습니다.

```swift
class PersonB {
    var name: String
    var age: Int
    var nickName: String
    // 이니셜라이저
    init(name: String, age: Int, nickName: String) {
        self.name = name
        self.age = age
        self.nickName = nickName
    }
}
let hana: PersonB = PersonB(name: "hana", age: 20, nickName: "하나")
```

> 프로퍼티의 초기값이 꼭 필요 없을 때

* 옵셔널을 사용

```swift
class PersonC {
    var name: String
    var age: Int
    var nickName: String?
    init(name: String, age: Int, nickName: String) {
        self.name = name
        self.age = age
        self.nickName = nickName
    }
    init(name: String, age: Int) {
        self.name = name
        self.age = age
    }
}
let jenny: PersonC = PersonC(name: "jenny", age: 10)
let mike: PersonC = PersonC(name: "mike", age: 15, nickName: "m")
```

* 암시적 추출 옵셔널은 인스턴스 사용에 꼭 필요하지만 초기값을 할당하지 않고자 할 때 사용

```swift
class Puppy {
    var name: String
    var owner: PersonC!
    init(name: String) {
        self.name = name
    }
    func goOut() {
        print("\(name)가 주인 \(owner.name)와 산책을 합니다")
    }
}
let happy: Puppy = Puppy(name: "happy")
// 강아지는 주인없이 산책하면 안돼요!
//happy.goOut() // 주인이 없는 상태라 오류 발생
happy.owner = jenny
happy.goOut()
// happy가 주인 jenny와 산책을 합니다
```

**실패가능한 이니셜라이저**

이니셜라이저 매개변수로 전달되는 초기값이 잘못된 경우 인스턴스 생성에 실패할 수 있습니다. 인스턴스 생성에 실패하면 nil을 반환합니다. 그래서 실패가능한 이니셜라이저의 반환타입은 옵셔널 타입입니다. init?을 사용합니다.

```swift
class PersonD {
    var name: String
    var age: Int
    var nickName: String?
    init?(name: String, age: Int) {
        if (0...120).contains(age) == false {
            return nil
        }
        if name.characters.count == 0 {
            return nil
        }
        self.name = name
        self.age = age
    }
}
//let john: PersonD = PersonD(name: "john", age: 23)
let john: PersonD? = PersonD(name: "john", age: 23)
let joker: PersonD? = PersonD(name: "joker", age: 123)
let batman: PersonD? = PersonD(name: "", age: 10)
print(joker) // nil
print(batman) // nil
```

**디이니셜라이저**

deinit은 클래스의 인스턴스가 메모리에서 해제되는 시점에 호출됩니다. 인스턴스가 해제되는 시점에 해야할 일을 구현할 수 있습니다. 자동으로 호출되므로 직접 호출할 수 없습니다. 인스턴스가 메모리에서 해제되는 시점은 ARC(Automatic Reference Counting) 의 규칙에 따라 결정됩니다. ARC에 대해 더 자세한 사항은 ARC 문서를 참고하세요.
디이니셜라이저는 클래스 타입에만 구현할 수 있습니다.

```swift
class PersonE {
    var name: String
    var pet: Puppy?
    var child: PersonC
    init(name: String, child: PersonC) {
        self.name = name
        self.child = child
    }
    // 인스턴스가 메모리에서 해제되는 시점에 자동 호출
    deinit {
        if let petName = pet?.name {
            print("\(name)가 \(child.name)에게 \(petName)를 인도합니다")
            self.pet?.owner = child
        }
    }
}
var donald: PersonE? = PersonE(name: "donald", child: jenny)
donald?.pet = happy
donald = nil // donald 인스턴스가 더이상 필요없으므로 메모리에서 해제됩니다
// donald가 jenny에게 happy를 인도합니다
```

---
출처 : 야곰 닷넷 - 스위프트 기초 [인스턴스의 생성과 소멸](https://yagom.net/courses/swift-basic/lessons/%ed%83%80%ec%9e%85-%ec%8b%ac%ed%99%94/topic/%ec%9d%b8%ec%8a%a4%ed%84%b4%ec%8a%a4%ec%9d%98-%ec%83%9d%ec%84%b1%ea%b3%bc-%ec%86%8c%eb%a9%b8/)
___

###### tags: `swift`
