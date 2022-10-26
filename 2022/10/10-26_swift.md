# 2022-10-26 Swift

## 기초 개념

### 이름짓기 규칙
* Lower Camel Case : function, method, variable, constant
    * ex) someVariableName

* Upper Camel Case : type(class, struct, enum, extension, ...)
    * ex) Person, Point, Week

* 대소문자 구분

### 콘솔로그
* print
    * 단순 문자열 출력

* dump
    * 인스턴스의 자세한 설명(description 프로퍼티)까지 출력

* 문자열 보간법
    * String Interpolation
    * 프로그램 실행 중 문자열 내에 변수 또는 상수의 실질적인 값을 표현하기 위해 사용
    * \\()

### 상수, 변수
* 상수
    * 상수 선언 키워드 let
    * let 이름: 타입 = 값
    * ex) let constant: String = "차후에 변경이 불가능한 상수 let"

* 변수
    * 변수 선언 키워드 var
    * var 이름: 타입 = 값
    * ex) var variable: String = "차후에 변경이 가능한 변수 var"

```swift
let constant: String = "차후 변경 불가능"
var variable: String = "차후 변경 가능"

variable = "변수는 이렇게 차후에 다른 값으로 변경 가능"
// constant = "상수는 차후에 다른 값으로 변경할 수 없음"

let sum: Int
let inputA: Int = 100
let inputB: Int = 200

// 선언 후 첫 할당
sum = inputA + inputB

// sum = 1 // 그 이후에는 다시 값을 바꿀 수 없습니다, 오류발생

// 변수도 물론 차후에 할당하는 것이 가능합니다
var nickName: String

nickName = "yagom"

// 변수는 차후에 다시 다른 값을 할당해도 문제가 없지요
nickName = "야곰"
```

---
출처 : 야곰 닷넷 - 스위프트 기초 [기초개념](https://yagom.net/courses/swift-basic/lessons/%ea%b8%b0%ec%b4%88%ea%b0%9c%eb%85%90/)
___

## 데이터 타입

### **Swift 기본 데이터 타입**

### Bool
* true와 false만을 값으로 가지는 타입
```swift
var someBool: Bool = true
someBool = false
// someBool = 0    // 컴파일 오류발생
// someBool = 1    // 컴파일 오류발생
```

### Int, UInt
* Int
    * 정수 타입, 현재는 기본적으로 64비트 정수형
```swift
var someInt: Int = -100
// someInt = 100.1    // 컴파일 오류발생
```

* UInt
    * 양의 정수 타입, 현재는 기본적으로 64비트 양의 정수형
```swift
var someUInt: UInt = 100
// someUInt = -100    // 컴파일 오류발생
// someUInt = someInt    // 컴파일 오류발생
```

### Float, Double
* Float
    * 실수 타입, 32비트 부동소수형
```swift
var someFloat: Float = 3.14
someFloat = 3
```

* Double
    * 실수 타입, 64비트 부동소수형
```swift
var someDouble: Double = 3.14
someDouble = 3
// someDouble = someFloat    // 컴파일 오류발생
```

### Character, String
* Character
    * 문자 타입, 유니코드 사용, 큰따옴표("")사용
```swift
var someCharacter: Character = "😀"
someCharacter = "👭"
someCharacter = "가"
someCharacter = "A"
// someCharacter = "하하하"    // 컴파일 오류발생
print(someCharacter)
```

* String
    * 문자열 타입, 유니코드 사용, 큰따옴표("")사용

```swift
var someString: String = "하하하 ? "
someString = someString + "웃으면 복이와요"
print(someString)

// someString = someCharacter // 컴파일 오류발생
```

> 여러줄 문자열은 큰따옴표 세 개 사용

```swift
someString = """
여러줄 문자열을
사용할 수 있습니다.
첫 줄에 겹따옴표 세 개,
마지막 줄에 겹따옴표 세 개를
사용하면 됩니다.
"""

someString = """
겹따옴표 세 개인 줄(첫줄, 끝줄)에서
줄 바꿈을 하지 않으면 오류가 발생합니다.
"""

/*
someString = """오류발생
오류발생"""
*/
```

---
출처 : 야곰 닷넷 - 스위프트 기초 [기본 데이터 타입](https://yagom.net/courses/swift-basic/lessons/%eb%8d%b0%ec%9d%b4%ed%84%b0-%ed%83%80%ec%9e%85/topic/%ea%b8%b0%eb%b3%b8-%eb%8d%b0%ec%9d%b4%ed%84%b0-%ed%83%80%ec%9e%85/)
___

## Any, AnyObject, nil

### Any
* Swift의 모든 타입을 지칭하는 키워드
    * Any타입에 Double 자료를 넣어두었더라도 Any는 Double타입이 아니기 때문에 할당할 수 없습니다. 명시적으로 타입을 변환해 주어야 합니다. 타입 변환은 차후에 다룹니다.

```swift
var someAny: Any = 100
someAny = "어떤 타입도 수용 가능합니다"
someAny = 123.12

let someDouble: Double = someAny    // 컴파일 오류발생
```

### AnyObject
* 모든 클래스 타입을 지칭하는 프로토콜

```swift
class SomeClass {}

var someAnyObject: AnyObject = SomeClass()
// AnyObject는 클래스의 인스턴스만 수용 가능하기 때문에 클래스의 인스턴스가 아니면 할당할 수 없습니다.
someAnyObject = 123.12    // 컴파일 오류발생
```

### nil
* 없음을 의미하는 키워드
    * 아래 코드에서 someAny는 Any 타입이고, someAnyObject는 AnyObject 타입이기 때문에 nil을 할당할 수 없습니다.
    * nil을 다루는 방법은 옵셔널 파트에서 다룹니다.

```swift
someAny = nil    // 컴파일 오류발생
someAnyObject = nil    // 컴파일 오류발생
```

---
출처 : 야곰 닷넷 - 스위프트 기초 [Any, AnyObject, nil](https://yagom.net/courses/swift-basic/lessons/%eb%8d%b0%ec%9d%b4%ed%84%b0-%ed%83%80%ec%9e%85/topic/any-anyobject-nil//)
___

## 컬렉션 타입

| **타입**  | **설명** |
| :-----: | :-----: |
| Array | **순서**가 있는 리스트 컬렉션 |
| Dictionary | **키**와 **값**의 쌍으로 이루어진 컬렉션 |
| Set | 순서가 없고, **멤버가 유일**한 컬렉션 |

### Array
* Array는 멤버가 순서(인덱스)를 가진 리스트 형태의 컬렉션 타입입니다.

> **Array 선언 및 생성**
> Array는 여러 리터럴 문법을 활용할 수 있어서 표현 방법이 다양합니다.
```swift
// 빈 Int Array 생성
var integers: Array<Int> = Array<Int>()

/*
같은 표현
var integers: Array<Int> = [Int]()
var integers: Array<Int> = []
var integers: [Int] = Array<Int>()
var integers: [Int] = [Int]()
var integers: [Int] = []
var integers = [Int]()
*/
```

> Array 활용
```swift
integers.append(1)
integers.append(100)
// integers.append(101.1)
// Int 타입이 아니므로 Array에 추가할 수 없습니다.

print(integers)    // [1, 100]

// 멤버 표함 여부 확인
print(integers.contains(100))    // true
print(integers.contains(99))    // false

// 멤버 교체
integers[0] = 99    // integers = [99, 100]

// 멤버 삭제
integers.remove(ar: 0)
integers.removeLast()
integers.removeAll()

// 멤버 수 확인
print(integers.count)

// 인덱스를 벗어나 접근하려면 익셉션 런타임 오류발생
// integers[0]
```
> let을 사용하여 Array를 선언하면 불변 Array가 됩니다
```swift
let immutableArray = [1, 2, 3]

// 수정이 불가능한 Array이므로 멤버를 추가하거나 삭제할 수 없습니다
//immutableArray.append(4)
//immutableArray.removeAll()
```

### Dictionary
* Dictionary는 키와 값의 쌍으로 이루어진 컬렉션 타입입니다.

> **Dictionary의 선언과 생성**
> Dictionary는 여러 리터럴문법을 활용할 수 있어서 표현 방법이 다양합니다.

```swift
// Key가 String타입이고 Value가 Any인 빈 Dictionary 생성
var anyDictionary: Dictionary<String, Any> = [String: Any]()

/*
같은 표현
var anyDictionary: Dictionary<String, Any> = Dictionary<String, Any>()
var anyDictionary: Dictionary<String, Any> = [:]
var anyDictionary: [String: Any] = Dictionary<String, Any>()
var anyDictionary: [String: Any] = [String: Any]()
var anyDictionary: [String: Any] = [:]
var anyDictionary = [String: Any]()
*/
```

> Dictionary 활용
```swift
anyDictionary["someKey"] = "Value"
anyDictionary["anotherKey"] = 100
print(anyDictionary) // ["someKey": "value", "anotherKey": 100]

// 키에 해당하는 값 변경
anyDictionary["someKey"] = "dictionary"
print(anyDictionary) // ["someKey": "dictionary", "anotherKey": 100]

// 키에 해당하는 값 제거
anyDictionary.removeValue(forKey: "anotherKey")
anyDictionary["someKey"] = nil
print(anyDictionary)
```

> let을 사용하여 Dictionary를 선언하면 불변 Dictionary가 됩니다.
```swift
let emptyDictionary: [String: String] = [:]
let initalizedDictionary: [String: String] = ["name": "yagom", "gender": "male"]

// 불변 Dictionary이므로 값 변경 불가
//emptyDictionary["key"] = "value"
```

> 키에 해당하는 값을 다른 변수나 상수에 할당하고자 할때는 옵셔널과 타입 캐스팅 파트에서 다룹니다
```swift
// "name"이라는 키에 해당하는 값이 없을 수 있으므로 
// String 타입의 값이 나올 것이라는 보장이 없습니다.
// 컴파일 오류가 발생합니다
let someValue: String = initalizedDictionary["name"]
```

### Set
* Set는 순서가 없고, 멤버가 유일한 것을 보장하는 컬렉션 타입입니다.

> Set의 선언과 생성
```swift
// 빈 Int Set 생성
var integerSet: Set<Int> = Set<Int>()
integerSet.insert(1)
integerSet.insert(100)
integerSet.insert(99)
integerSet.insert(99)
integerSet.insert(99)
```

> Set는 집합연산에 많이 활용됩니다
```swift
let setA: Set<Int> = [1,2,3,4,5]
let setB: Set<Int> = [3,4,5,6,7]

// 합집합
let union: Set<Int> = setA.union(setB)
print(union)    // [2,4,5,6,7,3,1]

// 합집합 오름차순 정렬
let sortedUnion: [Int] = union.sorted()
print(sortedUnion)    // [1,2,3,4,5,6,7]

// 교집합
let intersection: Set<Int> = setA.intersection(setB)
print(intersection)    // [5,3,4]

// 차집합
let subtracting: Set<Int> = setA.subtracting(setB)
print(subtracting)    // [2,1]
```

---
출처 : 야곰 닷넷 - 스위프트 기초 [컬렉션 타입](https://yagom.net/courses/swift-basic/lessons/%eb%8d%b0%ec%9d%b4%ed%84%b0-%ed%83%80%ec%9e%85/topic/%ec%bb%ac%eb%a0%89%ec%85%98-%ed%83%80%ec%9e%85/)
___

###### tags: `swift`
