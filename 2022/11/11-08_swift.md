# 2022-11-08 Swift

## 옵셔널 심화

### 옵셔널 체이닝

옵셔널 체이닝은 옵셔널의 내부의 내부의 내부로 옵셔널이 연결되어 있을 때 유용하게 활용할 수 있습니다. 매 번 nil 확인을 하지 않고 최종적으로 원하는 값이 있는지 없는지 확인할 수 있습니다.

**예제 클래스**

```swift
class Person {
    var name: String
    var job: String?
    var home: Apartment?
    init(name: String) {
        self.name = name
    }
}
class Apartment {
    var buildingNumber: String
    var roomNumber: String
    var `guard`: Person?
    var owner: Person?
    init(dong: String, ho: String) {
        buildingNumber = dong
        roomNumber = ho
    }
}
```

**옵셔널 체이닝 사용**

```swift
let yagom: Person? = Person(name: "yagom")
let apart: Apartment? = Apartment(dong: "101", ho: "202")
let superman: Person? = Person(name: "superman")
// 옵셔널 체이닝이 실행 후 결과값이 nil일 수 있으므로
// 결과 타입도 옵셔널입니다
// 만약 우리집의 경비원의 직업이 궁금하다면..?
// 옵셔널 체이닝을 사용하지 않는다면...
func guardJob(owner: Person?) {
    if let owner = owner {
        if let home = owner.home {
            if let `guard` = home.guard {
                if let guardJob = `guard`.job {
                    print("우리집 경비원의 직업은 \(guardJob)입니다")
                } else {
                    print("우리집 경비원은 직업이 없어요")
                }
            }
        }
    }
}
guardJob(owner: yagom)
// 옵셔널 체이닝을 사용한다면
func guardJobWithOptionalChaining(owner: Person?) {
    if let guardJob = owner?.home?.guard?.job {
        print("우리집 경비원의 직업은 \(guardJob)입니다")
    } else {
        print("우리집 경비원은 직업이 없어요")
    }
}
guardJobWithOptionalChaining(owner: yagom)
// 우리집 경비원은 직업이 없어요
yagom?.home?.guard?.job // nil
yagom?.home = apart
yagom?.home // Optional(Apartment)
yagom?.home?.guard // nil
yagom?.home?.guard = superman
yagom?.home?.guard // Optional(Person)
yagom?.home?.guard?.name // superman
yagom?.home?.guard?.job // nil
yagom?.home?.guard?.job = "경비원"
```

**nil 병합 연산자**

중위 연산자입니다. ??

Optional ?? Value

옵셔널 값이 nil일 경우, 우측의 값을 반환합니다. 띄어쓰기에 주의하여야 합니다.

```swift
var guardJob: String
guardJob = yagom?.home?.guard?.job ?? "슈퍼맨"
print(guardJob) // 경비원
yagom?.home?.guard?.job = nil
guardJob = yagom?.home?.guard?.job ?? "슈퍼맨"
print(guardJob) // 슈퍼맨
```

---
출처 : 야곰 닷넷 - 스위프트 기초 [옵셔널 체이닝과 nil 병합 연산자](https://yagom.net/courses/swift-basic/lessons/%ec%98%b5%ec%85%94%eb%84%90-%ec%8b%ac%ed%99%94/topic/%ec%98%b5%ec%85%94%eb%84%90-%ec%b2%b4%ec%9d%b4%eb%8b%9d%ea%b3%bc-nil-%eb%b3%91%ed%95%a9-%ec%97%b0%ec%82%b0%ec%9e%90/)
___

### 타입 캐스팅

스위프트의 타입캐스팅은 **인스턴스의 타입을 확인** 하는 용도 또는 클래스의 인스턴스를 부모 혹은 **자식 클래스의 타입으로 사용할 수 있는지 확인** 하는 용도로 사용합니다. is, as를 사용합니다.

**타입 캐스팅 예제를 위한 클래스 정의**

```swift
class Person {
    var name: String = ""
    func breath() {
        print("숨을 쉽니다")
    }
}
class Student: Person {
    var school: String = ""
    func goToSchool() {
        print("등교를 합니다")
    }
}
class UniversityStudent: Student {
    var major: String = ""
    func goToMT() {
        print("멤버쉽 트레이닝을 갑니다 신남!")
    }
}
// 인스턴스 생성
var yagom: Person = Person()
var hana: Student = Student()
var jason: UniversityStudent = UniversityStudent()
```

**타입 확인**

is를 사용하여 타입을 확인합니다.

```swift
var result: Bool
result = yagom is Person // true
result = yagom is Student // false
result = yagom is UniversityStudent // false
result = hana is Person // true
result = hana is Student // true
result = hana is UniversityStudent // false
result = jason is Person // true
result = jason is Student // true
result = jason is UniversityStudent // true
if yagom is UniversityStudent {
    print("yagom은 대학생입니다")
} else if yagom is Student {
    print("yagom은 학생입니다")
} else if yagom is Person {
    print("yagom은 사람입니다")
} // yagom은 사람입니다
switch jason {
case is Person:
    print("jason은 사람입니다")
case is Student:
    print("jason은 학생입니다")
case is UniversityStudent:
    print("jason은 대학생입니다")
default:
    print("jason은 사람도, 학생도, 대학생도 아닙니다")
} // jason은 사람입니다
switch jason {
case is UniversityStudent:
    print("jason은 대학생입니다")
case is Student:
    print("jason은 학생입니다")
case is Person:
    print("jason은 사람입니다")
default:
    print("jason은 사람도, 학생도, 대학생도 아닙니다")
} // jason은 대학생입니다
```

**업 캐스팅**

as를 사용하여 부모클래스의 인스턴스로 사용할 수 있도록 컴파일러에게 타입정보를 전환해줍니다. Any 혹은 AnyObject로도 타입정보를 변환할 수 있습니다. 암시적으로 처리되므로 꼭 필요한 경우가 아니라면 생략해도 무방합니다.

```swift
// UniversityStudent 인스턴스를 생성하여 Person 행세를 할 수 있도록 업 캐스팅
var mike: Person = UniversityStudent() as Person
var jenny: Student = Student()
//var jina: UniversityStudent = Person() as UniversityStudent // 컴파일 오류
// UniversityStudent 인스턴스를 생성하여 Any 행세를 할 수 있도록 업 캐스팅
var jina: Any = Person() // as Any 생략가능
```

**다운 캐스팅**

as? 또는 as!를 사용하여 자식 클래스의 인스턴스로 사용할 수 있도록 컴파일러에게 인스턴스의 타입정보를 전환해줍니다.

**조건부 다운 캐스팅**

as?를 사용합니다. 캐스팅에 실패하면, 즉 캐스팅하려는 타입에 부합하지 않는 인스턴스라면 nil을 반환하기 때문에 결과의 타입은 옵셔널 타입입니다.

```swift
var optionalCasted: Student?
optionalCasted = mike as? UniversityStudent
optionalCasted = jenny as? UniversityStudent // nil
optionalCasted = jina as? UniversityStudent // nil
optionalCasted = jina as? Student // nil
```

**강제 다운 캐스팅**

as!를 사용합니다. 캐스팅에 실패하면, 즉 캐스팅하려는 타입에 부합하지 않는 인스턴스라면 런타임 오류가 발생합니다. 캐스팅에 성공하면 옵셔널이 아닌 일반 타입을 반환합니다.

```swift
var forcedCasted: Student
forcedCasted = mike as! UniversityStudent
//forcedCasted = jenny as! UniversityStudent // 런타임 오류
//forcedCasted = jina as! UniversityStudent // 런타임 오류
//forcedCasted = jina as! Student // 런타임 오류
```

**활용**

```swift
func doSomethingWithSwitch(someone: Person) {
    switch someone {
    case is UniversityStudent:
        (someone as! UniversityStudent).goToMT()
    case is Student:
        (someone as! Student).goToSchool()
    case is Person:
        (someone as! Person).breath()
    }
}
doSomethingWithSwitch(someone: mike as Person) // 멤버쉽 트레이닝을 갑니다 신남!
doSomethingWithSwitch(someone: mike) // 멤버쉽 트레이닝을 갑니다 신남!
doSomethingWithSwitch(someone: jenny) // 등교를 합니다
doSomethingWithSwitch(someone: yagom) // 숨을 쉽니다
func doSomething(someone: Person) {
    if let universityStudent = someone as? UniversityStudent {
        universityStudent.goToMT()
    } else if let student = someone as? Student {
        student.goToSchool()
    } else if let person = someone as? Person {
        person.breath()
    }
}
doSomething(someone: mike as Person) // 멤버쉽 트레이닝을 갑니다 신남!
doSomething(someone: mike) // 멤버쉽 트레이닝을 갑니다 신남!
doSomething(someone: jenny) // 등교를 합니다
doSomething(someone: yagom) // 숨을 쉽니다
```

---
출처 : 야곰 닷넷 - 스위프트 기초 [타입 캐스팅](https://yagom.net/courses/swift-basic/lessons/%ec%98%b5%ec%85%94%eb%84%90-%ec%8b%ac%ed%99%94/topic/%ed%83%80%ec%9e%85%ec%ba%90%ec%8a%a4%ed%8c%85/)
___

###### tags: `swift`
