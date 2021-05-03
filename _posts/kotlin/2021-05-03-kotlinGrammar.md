---
title: "[Kotlin] Kotlin 문법 정리"
header:
overlay_color: "#333"
categories:
- Kotlin
  tag:
- kotlin
  toc: true
  toc_sticky: true
  comments: true
---


**코틀린 문법 정리**   



# 1. 기본 프로젝트 구조

- 코틀린은 자바와 달리 폴더 구조와 패키지 명을 일치시키지 않아도 된다.   
  단순히 파일 상단에 패키지만 명시하면 컴파일러가 알아서 처리한다.  
  그런데 같은 패키지 내에서는 변수, 함수, 클래스를 공유할 수 있는데, 패키지가 다르면 쓸 수 없다.  
  => import 를 통해 가능함.

- 코틀린은 클래스명과 파일명이 일치하지 않아도 되며 하나의 파일에 여러개의 클래스를 넣어도 알아서
  컴파일이 가능하다.

**이는 파일이나 폴더를 기준으로 구분하지 않고 파일내에 있는 package 키워드를 기준으로 구분하기 때문**  


# 2장. 변수와 자료형

## 1. 주석
- //
- /* */
- 세미콜론을 붙이지 않아도 된다.

## 2. 변수 선언
- val
    - 선언시에만 초기화 가능. 중간에 값을 변경할 수 없음.
    - 런타임 시 변경되지 말아야 할 값을 선언할 떄 사용하는 것이 좋음.
- var
    - 일반적으로 통용되는 변수. 언제든지 읽기 쓰기가 가능함.

- 변수는 선언 위치에 따라 두가지 이름으로 불림.
    - Property(속성) : 클래스에 선언된 변수
    - Local Variable(로컬변수) : 이 외의 Scope 내에 선언된 변수

- 코틀린 vs 고전적인 언어들
    - 고전적인 언어 : 변수가 선언된 후 초기화 되지 않으면 기본값으로 초기화 되거나 값이 할당
      되지 않았다는 표시로 null 값을 가지게 됨.
    - 코틀린 : 기본 변수에서 null을 허용하지 않으며 또한 변수에 값을 할당하지 않은채로
      사용하게 되면 문법 에러를 표시하고 컴파일을 막아준다. 따라서 의도치 않은 동작이나
      null pointer exception 등을 원천적으로 차단해 준다는 장점이 있음

- 프로그램에 따라서 변수에 값이 할당되지 않았다는 것을 하나의 정보로 사용하는 경우도 있음  
  이런 경우에는 자료형 뒤에 물음표를 붙이면 null을 허용하는 nullable 변수로 선언해 줄 수 있음.
```kotlin
var a: Int ? = null
```    

- nullable 변수는 null인 상태로 연산할 시 null pointer exception이 발생할 수
  있으므로 꼭 필요한 경우에 한해 주의해서 사용해야 한다.   
  이 외에도 변수의 초기화를 늦추는 lateinit이나 lazy 속성도 있으나 이는 클래스에 대한 지식이
  필요하므로 이후에 언급할 예정.

- 그럼 변수에서 사용할 수 있도록 코틀린이 제공하는 기본 자료형에는 어떤것들이 있을까?


# 3장. 형변환과 배열

## 1. 기본 자료형(primitive type)
이 부분은 자바와의 호환을 위해 자바와 거의 동일함.
- 숫자형
    - 정수형(byte, short, int, long)
    - 실수형(float, double)
    - 사용하고자 하는 숫자의 크기에 따라 선택해서 사용하면 됨
    - 정수형의 리터럴(리터럴 : 코드 내에 값을 표기하는 것) : 10진수, 16진수, 2진수
```kotlin
fun main() {
    var intValue: Int = 1234
    var longValue: Long = 1234L
    var intValueByHex: Int = 0x1af
    var intValueByBin: Int = 0b10110110
    // 참고로 코틀린은 8진수 표기는 지원하지 않는다.

    var doubleValue: Double = 123.5
    var floatValue: Float = 123.5f
}
```
- 문자형  
  코틀린은 내부적으로 문자열을 유니코드 인코딩 중에 한 방식인 UTF-16 BE 로 관리함.
  따라서 글자 하나하나가 2bytes(16bits)의 메모리 공간을 사용한다.
    - Char : 1개의 문자
        - Char의 리터럴 : 작은 따옴표로 표기
    - 논리형 : boolean(true/false)
        - 논리형의 리터럴 : true false
    - 문자열
        - 따옴표 내에 문자열 작성


```kotlin
    var charValue: Char = 'a' // 문자 
    var koreanCharValue: Char = '가' // 문자 

    var booleanValue: Boolean = true // 논리

    val stringValue = "one line string test" // 문자열 
    val multiLineStringValue = """multiline
    string
    test
    """
```

- 형변환 함수
```kotlin
toByte()
toShort()
toInt()
toLong()
toFloat()
toDOuble()
toChar()
```

```kotlin
fun main() {
    var a:Int = 12345
    var b:Long = a.toLong() // 명시적 형변환 
    // 참고로 코틀린은 형변환시 발생할 수 있는 오류를 막기 위해 다른 언어들이 지원하는 '암시적 형변환'은 지원하지 않는다. 
}
```  
- 명시적 형변환
    - 변환된 자료형을 개발자가 직접 지정함
- 암시적 형변환
    - 변수를 할당할 시 자료형을 지정하지 않아도 자동으로 형변환 됨
- 형 변환시 호환이 가능한지 여부를 체크하여 변환여부를 확인할 수 있는 방법은 클래스와 관련될 것(추후에 언급할 예정)


## 2. 배열
배열은 내부적으로 Array<T> 클래스로 제공되는 기능

```kotlin
fun main() {
    var intArr = arrayOf(1,2,3,4,5)
    var nullArr = arrayOfNulls<Int>(5) // 특정한 크기를 가지는 비어있는 배열  
}
```

- 사용법
    - 배열에 값을 할당하거나 사용 -> 다른 언어와 비슷 ex) intArr[2]

- 배열은 처음 선언했을 떄의 전체크기를 변경할 수 없다는 단점이 있지만 한 번 선언을 해두면 다른 자료구조보다 빠른 입출력이
  가능하다는 장점이 있다.  


# 4장. 타입추론과 함수

## 1. 타입추론
변수나 함수들을 선언할 때나 연산이 이루어질 때 자료형을 코드에 명시하지 않아도 코틀린이 자동으로
자료형을 추론해주는 기능

```kotlin

val stringValue = "test" // 1
val stringValue: String = "test" // 1

var intArr = arrayOf(1,2,3,4,5) // 2
var intArr: Array<Int> = arrayOf(1,2,3,4,5) // 2
``` 

- 반드시 특정한 자료형으로 지정해야하는 상황이 아니라면 대부분은 코틀린의 타입추론 기능을
  이용하여 코드량을 줄일 수 있음


## 2. 함수
특정한 동작을 하거나 원하는 결과값을 연산하는데 사용
```kotlin
fun main() {
    println(add(5,6,7))
}
fun add(a:Int, b:Int, c:Int):Int {
     return a+b+c
}
fun add(a:Int, b:Int, c:Int) = a+b+c // 단일 표현식 함수  
```


# 5장. 비교연산자와 반복문

## 1. 비교연산자
- is 연산자 : 자료형이 맞는지 체크
- !is 연산자 : 자료형이 틀린지 체크

```kotlin
fun main() {
    var a = 7
    a is Int // 좌측 변수가 우측 자료형에 호환되는지 여부를 체크하고 형변환까지 한번에 진행. 
}
```

- when 연산자
  switch 기능을 좀 더 편리하게 바꾼 기능
    - if가 참과 거짓만을 비교할 수 있는 반면 when은 하나의 변수를 여러개의 값과 비교할 수 있다는 장점이 있음

```kotlin
fun main() {
    doWhen(1)
    doWhen("KJY")
    doWhen(12L)
    doWhen(3.12345)
    doWhen("Kotlin")

}

fun doWhen(a: Any) {
    when(a) {
        1 -> println("정수 1입니다")
        "KJY" -> println("김진영")
        is Long -> println("Long type")
        !is String -> println("Not String type")
        else -> println("어떤 조건도 만족하지 않습니다. ")
    }
}
```    

```text
// 결과 
정수 1입니다
김진영
Long type
Not String type
어떤 조건도 만족하지 않습니다. 
```

## 2. 반복문
- 조건형 반복문 : 조건이 참인 경우 반복을 유지
    - while, do~while
```kotlin
fun main() {;
    var a = 0
    
    // while
    while(a<5) {
        println(a++) // 0 1 2 3 4
    }
    
    // do~while
    do 
    {
        println(a++)
    } while(a<5)  
    
}
// 조건과 관계없이 반드시 '한번은 실행'해야 한다면 do...while문을 사용하는 것이 좋을 것임. 
```    
- 범위형 반복문 : 반복 범위를 정해 반복을 수행
    - for loop
```kotlin
fun main() {
    for(i in 0..9) {
        print(i) // 줄을 바꾸지 않고 붙여서 출력하는 방식 = print()
    }
}
```
```text
// 결과 
0123456789
```

```kotlin
fun main() {
    for(i in 0..9 step 3) {
        print(i) // 줄을 바꾸지 않고 붙여서 출력하는 방식 = print()
    }
}
```
```text
0369
```

```kotlin
fun main() {
    for(i in 9 downTo 0) {
        print(i) // 줄을 바꾸지 않고 붙여서 출력하는 방식 = print()
    }
}
```
```text
9876543210
```

```kotlin
fun main() {
    for(i in 'a'..'e') {
        print(i) // 줄을 바꾸지 않고 붙여서 출력하는 방식 = print()
    }
}
```
```text
abcde
```

# 6장. 흐름제어와 논리연산자

```kotlin
fun main() {
    for(i in 1..10) {
        for (j in 1..10) {
            if (i==1 && j ==2) break
        }
        // 조건문으로 또 체크해서 종료해야 함 
    }
}
```

```kotlin
// loop@를 사용하면 재체크할 필요 없이 레이블이 달린 반복문을 기준으로 즉시 break를 시켜줌 
fun main() {
    loop@for(i in 1..10) {
        for (j in 1..10) {
            if (i==1 && j ==2) break@loop
        }
    }
}
```


# 7. 클래스
## 구조
- 속성(고유의 특징값) + 함수(기능의 구현)
```kotlin
fun main() {
    var a = Person("박보영", 1990)
    var b = Person("전정국", 1997)
    var c = Person("장원영", 2004)
    // 사용법 = 변수명.속성명(.: 참조 연산자)
    // ex) a.name
    
    // 속성만 
    println("안녕하세요. ${a.birthYear}년생 ${a.name}입니다. ")
    
    // 속성 + 함수 
    a.introduce()
}

// 함수 없이 속성만 갖춘 클래스(변수 선언과 형태가 같음)
// 클래스의 '속성'들을 선언함과 동시에 '생성자' 역시 선언하는 방법 
class Person(var name:String, val birthYear:Int) 


// 속성 + 함수로 이루어져 있는 클래스
class Person(var name:String, val birthYear:Int) {
    fun introduce() {
        println("안녕하세요. ${birthYear}년생 ${name}입니다. ")
    }
}

```

## 생성자
- init 함수 : 파라미터나 반환형이 없는 특수 함수로, 생성자를 통해 인스턴스가 만들어 질 때 호출되는 함수

- 기본 생성자
    - 클래스를 만들 때 기본으로 선언
- 보조 생성자
    - 필요에 따라 추가적으로 선언
    - 기본 생성자와 다른 형태의 생성자를 제공하여 인스턴스 생성시 편의를 제공하거나 추가적인 구문을 수행하는 기능을 제공하는 역할을 한다.
    - constructor() 키워드 사용


```kotlin
fun main() {
    var a = Person("박보영", 1990)
    var b = Person("전정국", 1997)
    var c = Person("장원영", 2004)
    
    var d = Person("이루다")
    var e = Person("차은우")
    var f = Person("류수정")
}

class Person(var name:String, val birthYear:Int) {
    init {
        println("${this.birthYear}년생 ${this.name}님이 생성되었습니다.")
    }
    
    // 주의점 : 반드시 기본 생성자를 통해 속성을 초기화 해줘야 함 
    // 보조 생성자가 기본 생성자를 호출하도록 하려면 콜론(:)을 붙인 후 
    // this라는 키워드 사용하고 파라미터를 괄호 안에 넣기 
    // ex) 100명 중 90명이 1997년생인 경우 (디폴트값을 설정하는 경우) 
    constructor(name:String) : this(name, 1997) {
        println("보조 생성자가 사용되었습니다.")
    }
}
```
```text
// 결과 
1990년생 박보영님이 생성되었습니다.
1997년생 전정국님이 생성되었습니다.
2004년생 장원영님이 생성되었습니다.
1997년생 이루다님이 생성되었습니다.
보조 생성자가 사용되었습니다.
1997년생 차은우님이 생성되었습니다.
보조 생성자가 사용되었습니다.
1997년생 류수정님이 생성되었습니다.
보조 생성자가 사용되었습니다.
```

## 상속
- 상속이 필요한 경우 2가지
    - 이미 존재하는 클래스를 호출하여 새로운 속성이나 함수를 추가한 클래스를 만들어야 할 때
    - 여러개 클래스를 만들었는데 클래스의 공통점을 뽑아 코드 관리를 편하게 할 때

- 수퍼 클래스 : 속성과 함수를 물려주는 쪽
- 서브 클래스 : 물려받는 쪽
    - 수퍼 클래스에 존재하는 속성과 '같은 이름'의 속성을 가질 수 없음
    - 서브 클래스가 생성될 때는 반드시 수퍼클래스의 생성자까지 호출되어야 함

```kotlin
fun main() {
    var a = Animal("별이", 5, "개")
    var b = Dog("별이", 5)
  
  a.introduce()
  b.introduce()
  
  b.bark()
}

// *open : 클래스가 상속될 수 있도록 클래스 선언시 붙여줄 수 있는 키워드 
open class Animal (var name:String, var age:Int, var type:String)
{
    fun introduce() {
        println("저는 ${type} ${name}이고, ${age}살 입니다.")
    }
}

// var, val 등을 붙이면 속성으로 선언됨 
// 상속 : 콜론(:)을 붙이고 수퍼클래스의 생성자를 호출 
// var을 붙이지 말고 일반 파라미터로 받음 -> Animal 클래스의 생성자에 직접 넘겨줌
class Dog (name:String, age:Int) : Animal(name,age,"개") 
{
    fun bark() {
        println("멍멍")
    }
}
   
```

```text
// 결과 
저는 개 별이이고, 5살 입니다.
저는 개 별이이고, 5살 입니다.
멍멍
```

# 8. 오버라이딩과 추상화

## 오버라이딩

- 수퍼클래스에서 'open'이 붙은 함수는 서브클래스에서 'override'를 붙여 재구현하면 됨.

```kotlin
// 이미 수퍼클래스에서 구현이 끝난 함수를 오버라이딩을 통해 재구현한 경우  
fun main() {
    var t = Tiger()
    t.eat() // 출력 : 고기를 먹습니다.
}
open class Animal() { // 수퍼클래스 
    open fun eat() {
        println("음식을 먹습니다.")
    }
}

class Tiger : Animal() {  // 서브클래스 
    override fun eat() {
        println("고기를 먹습니다.")
    }
}
```

## 추상화
오버라이딩과 다르게 수퍼클래스에서는 함수의 구체적인 구현은 없고 단지 Animal의 모든 서브클래스는 eat이라는 함수가 '반드시 있어야 한다'는
점만 명시하여 각 서브클래스가 비어있는 함수의 내용을 필요에 따라 구현하도록 하려면 추상화라는 개념을 사용.

- 추상화란 선언부만 있고 기능이 구현되지 않은 추상함수, 그리고 추상함수를 포함하는 추상클래스라는 요소로 구성됨

```kotlin
fun main() {
    var r = Rabbit()
    r.eat()
    r.sniff()

}

abstract class Animal() {
    abstract fun eat()  // 미완성 함수이기 때문에 (abstract) 반드시 서브클래스에서 구현을 해줘야 함 
    fun sniff() {
        println("킁킁")
    }
}

class Rabbit : Animal() {
    override fun eat() {
        println("당근을 먹습니다.")
    }
}
```

- 인터페이스 : 속성 + 추상함수 + 일반함수

추상함수는 생성자를 가질 수 있는 반면, 인터페이스는 생성자를 가질 수는 없으며,  
인터페이스에서 구현부가 있는 함수는 open 함수로 간주하고,  
구현부가 없는 함수는 abstract 함수로 간주한다.   
**따라서 open, abstract 등 별도의 키워드가 없어도 포함된 모든 함수를 서브클래스에서 구현 및 재정의가 가능하다.**

또한 한번에 여러 인터페이스를 상속받을 수 있으므로 좀 더 유연한 설계가 가능하다.

```kotlin
fun main() {
    var d = Dog()
    d.run()
    d.eat()
    // 결과 
    // 우다다다 뜁니다.
    // 음식을 먹습니다.
}

interface Runner {
    fun run()
}

interface Eater {
    fun eat() {
        println("음식을 먹습니다.")
    }
}

class Dog : Runner, Eater {
    override fun run() {
        println("우다다다 뜁니다.")
    }
}
```

- 주의
    - '여러개'의 인터페이스나 클래스에서 같은 이름과 형태를 가진 함수를 구현하고 있다면 서브클래스에서는 혼선이 일어나지 않도록
      반드시 오버라이딩하여 재구현 해주어야 한다.  



# 9. 변수,함수,클래스의 접근범위와 접근제한자

- 변수나 함수, 클래스의 '공용범위'를 제어하는 단위인 **스코프**
- 스코프 외부에서 스코프 내부로의 접근을 제어하는 **접근제한자**

- scope
    - 언어차원에서 변수나 함수, 클래스 같은 '멤버'들을 서로 공유하여 사용할 수 있는
      범위를 지정해 둔 단위.
    - 스코프가 지정되는 범위 : 패키지내부, 클래스내부, 함수내부,,
    - 세가지 규칙
        - 스코프 외부에서는 스코프 내부의 멤버를 '참조연산자'로만 참조가 가능  
          즉, 클래스의 멤버를 참조할 때 클래스 외부에서 인스턴스명에 참조연산자(.)을 사용하여 접근함.
        - 동일 스코프 내에서는 멤버들을 '공유'할 수 있다
        - 하위 스코프에서는 상위 스코프의 멤버를 재정의 할 수 있다.  
          원래 스코프의 같은 레벨에서는 아래와 같이 같은 이름의 멤버를 만들어서는 안된다.   
          하지만 하위 스코프에서는 같은 이름의 멤버를 만들어 사용할 수 있다.
```kotlin
var a = "너두나두"
var a = "야나두" // conflicting declarations 발생 
```

## 접근제한자
스코프 외부에서 스코프 내부에 접근할 때 그 권한을 '개발자가 제어'할 수 있는 기능
- public, internal, private, protected
    - 변수, 함수, 클래스 선언 시 맨 앞에 붙여 사용
```kotlin
private var a = "..."
public fun b {...}
internal class C {...}
```
- 상황에 따라 두가지 경우로 기능이 나뉨
- 패키지 스코프
    - public : 어떤 패키지에서도 접근 가능
    - internal : 같은 모듈 내에서만 접근 가능
    - private : 같은 파일 내에서만 접근 가능
    - protected는 사용하지 않음
- 클래스 스코프
    - public : 클래스 외부에서 늘 접근 가능
    - private : 클래스 내부에서만 접근 가능
    - protected : 클래스 자신과 상속받은 클래스에서 접근 가능
    - internal은 사용하지 않음

스코프는 멤버들의 가용 범위를 지정해 둔 단위로 개발자는 의도에 따라 스코프 안에 변수나 함수, 클래스를 배치할 수 있으며
접근 제한자는 이러한 스코프의 외부와 내부에서 사용할 멤버를 분리하여 스코프 외부에서 건드리지 말아야 할 기능이나 값들을
안전하게 제한하는 용도를 가지고 있다. 


# 10. 고차함수와 람다함수

## 고차함수
함수를 마치 클래스에서 만들어 낸 인스턴스처럼 취급하는 방법  
함수를 '패러미터'로 넘겨 줄 수도 있고 '결과값으로 반환'받을 수도 있는 방법

코틀린에서는 모든 함수를 고차함수로 사용 가능하다.

```text
(패러미터) -> 반환형
(자료형, 자료형, ...) -> 자료형  
```

```kotlin
fun main() {
    b(::a) // 일반 함수를 고차 함수로 변경해주는 연산자 `::`
}
fun a (str: String) {
    println("$str 함수 a")
}

// 반환값이 없는 경우 Unit 을 사용 
fun b(function: (String) -> Unit) {
    function("b가 호출한")
}
// main 함수가 a 함수를 b 함수에 파라미터로 넘겼고, b 함수는 받아온 a 함수에 "b가 호출한"이라는 값을 넘겨서
// 호출하였다. 최종적으로 a 함수가 실행되면서 'b가 호출한 함수 a'가 출력된다.
```

## 람다함수
람다함수는 일반함수와 달리 그 자체가 고차함수이기 때문에 별도의 연산자 없이도 변수에 담을 수 있다.

```kotlin
fun main() {
    val c : (String) -> Unit = { str -> println("$str 람다함수")}
    // 아래와 같이 사용할 수도 있음  
    // val c = {str: String -> println("$str 람다함수")}
    // (String) -> Unit 자료형으로 저장됨 
    b(c)
}

// 반환값이 없는 경우 Unit 을 사용 
fun b(function: (String) -> Unit) {
    function("b가 호출한")
}

// 출력 :  b가 호출한 람다함수 
```

- 고차함수와 람다함수를 사용하면 이렇게 함수를 일종의 변수로 사용할 수 있다는 편의성도 있지만,
  이후 배우게 될 컬렉션의 조작이나 스코프 함수의 사용에도 도움이 된다. 


# 11. 스코프 함수

## 람다함수
- 람다함수도 여러 구문의 사용이 가능
- 람다함수에 파라미터가 없다면? 실행할 구문들만 나열하면 됨.
- 파라미터가 하나뿐이라면 it 사용
```kotlin
// 파라미터가 있는 예제 
fun main() {
    val c: (String) -> Unit = { str ->
        println("$str 람다함수")
        println("여러 구문을")
        println("사용가능합니다.")

    }
}
```
```kotlin
// 파라미터가 없는 예제 
fun main() {
    val a:() -> Unit = {println("파라미터가 없어요.")}
    val a:() -> Unit = {println("파라미터가 없어요.")}
}
```
```kotlin
// 파라미터가 하나인 예제 
fun main() {
    val c:(String) -> Unit = { println("$it 람다함수")}
}
```

## 스코프 함수
- 함수형 언어의 특징을 좀 더 편리하게 사용할 수 있도록 기본 제공하는 함수들
- 클래스에서 생성한 인스턴스를 scope 함수에 전달하면 인스턴스의 속성이나 함수를 좀 더 깔끔하게
  불러 쓸 수 있다.
- apply, run, with, also, let 가 있다.

### apply
인스턴스를 생성한 후 변수에 담기 전에 초기화 과정을 수행할 때 많이 쓰임    
main함수와 '별도의 scope'에서 인스턴스의 변수와 함수를 조작하므로 코드가 깔끔해진다는 장점이 있다.

```kotlin
fun main() {
    var a = Book("디모의 코틀린", 10000)
    a.name = "[초특가]" + a.name
    a.discount()

    // 위의 코드에서 apply 적용 
    var a = Book("디모의 코틀린", 10000).apply {
        name = "[초특가]" + name
        discount()
    }
}

class Book(var name: String, var price:Int)
{
fun discount() {
    price -= 2000
    println("할인 상품입니다.")
}
}
```
```text
// 결과 
할인 상품입니다.
[초특가]디모의 코틀린
8000
```

### run
apply 처럼 run 스코프 안에서 참조연산자를 사용하지 않아도 된다는 점은 같지만 일반 람다함수처럼
인스턴스 대신 마지막 구문의 결과값을 반환한다.  
따라서 이미 인스턴스가 만들어진 후에 인스턴스의 함수나 속성을 scope내에서 사용해야 할 때 유용함.

```kotlin
fun main() {
    var a = Book("디모의 코틀린", 10000).apply {
        name = "[초특가]" + name
        discount()
    }
  
    // run 함수로 값 출력 
    a.run {
        println("상품명: ${name}, 가격: ${price}원")
    }
}

class Book(var name: String, var price:Int)
{
fun discount() {
    price -= 2000
}
}
```
```text
// 결과 
상품명: [초특가]디모의 코틀린, 가격: 8000원
```


### with
run과 동일한 기능을 가지지만 단지 인스턴스를 참조연산자 대신 파라미터로 받는다는 차이뿐
```kotlin
// 차이가 진짜 이것뿐임
a.run{...}
with(a) {...}
```

### also/let
- 처리가 끝나면 인스턴스를 반환
    - apply/also

- 처리가 끝나면 최종값을 반환
    - run/let

다만 한가지 공통적인 '차이점'이 있다.
apply와 run은 참조연산자 없이 인스턴스의 변수와 함수를 사용할 수 있음.
also와 let은 마치 파라미터로 인스턴스를 넘긴것처럼 it을 통해서 인스턴스를 사용할 수 있음.

이 두 함수는 왜 굳이 파라미터를 통해서 인스턴스를 사용하는 귀찮은 과정을 거칠까?  
이는 같은 이름의 변수나 함수가 scope 바깥에 중복되어 있는 경우에 혼란을 방지하기 위해서 이다.
```kotlin
fun main() {
    var price = 5000

    var a = Book("디모의 코틀린", 10000).apply {
        name = "[초특가]" + name
        discount()
    }
    a.run {
        println("상품명: ${name}, 가격: ${price}원")
    }
   
    // run 대신 let으로 대체할 것. 
    a.let {
        println("상품명: ${it.name}, 가격: ${it.price}원")
    }
}

class Book(var name: String, var price:Int)
{
fun discount() {
    price -= 2000
}
}
// 출력 
// 상품명: [초특가]디모의 코틀린, 가격: 5000원
// 상품명: [초특가]디모의 코틀린, 가격: 8000원
```
- 여기서 왜 가격이 5000원 인가?
    - 이는 run 함수가 인스턴스 내의 price 속성보다 run 이 속해있는 'main함수'의 price 변수를
      우선시하고 있기 때문이다.  이럴 때는 run을 대체하는 let을 사용할 것.   
      apply역시 같은 경우가 있다면 also로 대체하여 사용하면 됨.


- scope 함수는 인스턴스의 속성이나 함수를 scope내에서 깔끔하게 분리하여 사용할 수 있다는 점 때문에
  코드의 가독성을 향상시킨다는 장점이 있다. 