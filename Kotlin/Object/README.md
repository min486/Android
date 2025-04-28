<div align="center">
  <p>
    <img src="../README.assets/kotlin-hero.png">
  </p>
  <br>
  <h2>Kotlin</h2>
  <p>코틀린 관련 내용 정리</p>
  <br>
  <br>
</div>

## 🔥 Object와 Companion Object

### 싱글턴 패턴 (Singleton Pattern)

- 클래스의 인스턴스를 오직 하나만 생성하도록 하는 패턴

- 일반적으로 클래스는 객체를 여러개 생성할 수 있지만, 싱글턴 패턴이 적용된 경우

  인스턴스는 하나만 생성되며, 여러곳에서 같은 인스턴스를 공유한다

<br>

### Object

- 코틀린에서는 object 키워드를 통해 싱글턴 패턴을 구현할 수 있다
- 별도로 객체를 생성하지 않고 즉시 사용할 수 있다

```kotlin
fun main(args: Array<String>) {
    // 일반 클래스는 객체를 각각 생성
    val car1 = Car()
    val car2 = Car()
  	println(car1 == car2)  // false (다른 객체)
  
    // Object는 전체 프로그램에서 하나의 인스턴스만 존재
    println(MyObject.number)  // 1
    MyObject.sayHello()  // Hello
}

object MyObject {
    val number = 1
  
    fun sayHello() {
        println("Hello")
    }
}
```

<br>

### Object 특징

- 싱글턴 인스턴스를 생성할 수 있다
- 생성자 사용 불가
- 프로퍼티, 메서드, 초기화 블록을 포함할 수 있다
- 다른 클래스나 인터페이스를 상속할 수 있다

<br>

### Companion Object

- 클래스 내부에 선언하는 object
- 클래스의 인스턴스를 생성하지 않고도, 클래스명만으로 멤버에 접근할 수 있게 해준다

```kotlin
fun main(args: Array<String>) {
    // companion object 내부 멤버에 바로 접근 가능
    println(Dinner.MENU)  // pasta
    Dinner.eatDinner()  // Today's Menu is pasta
  
    // 클래스 인스턴스 멤버에는 접근 불가
    println(Dinner.lunch)  // 오류 발생
}

class Dinner {
    val lunch = "steak"
  
    companion object {
        val MENU = "pasta"
        fun eatDinner() {
            println("Today's Menu is $MENU")
        }
    }
}
```

<br>

### Companion Object 특징

- 한 클래스에는 하나의 companion object만 선언할 수 있다

- 이름을 붙일 수도 있고, 생략할 수도 있다
- object 키워드와 비슷한 기능을 하지만, 클래스 외부에서 바로 접근할 수 있다

```kotlin
class Example {
    companion object NamedCompanion {
        const val CONSTANT = 42
    }
}

// 접근 방법
println(Example.CONSTANT)  // 일반 접근
println(Example.NamedCompanion.CONSTANT)  // 이름으로 접근도 가능
```



<br>

### Object / Companion Object 차이

- Object : 코틀린의 싱글턴 패턴 구현 키워드. 프로그램 전체에서 하나만 존재
- Compaion Object : 클래스 내부에서 선언하는 object. 인스턴스 없이 클래스명으로 멤버에 접근할 수 있게 해준다

| 구분      | Object               | Companion Object          |
| --------- | -------------------- | ------------------------- |
| 위치      | 클래스 외부          | 클래스 내부               |
| 생성 시점 | 프로그램 시작시 생성 | 클래스 로딩시 생성        |
| 접근 방법 | 객체명.변수명        | 클래스명.변수명           |
| 특징      | 일반 싱글턴 객체     | 클래스 소속의 싱글턴 객체 |

<br>

접근 방법 비교

1. Object

```kotlin
// 예제 1
class DataSourceClass {
    object Datasource {
        var baseUrl: String = "google.com"
      
        fun printBaseUrl() {
            println("baseUrl is : $baseUrl")
        }
    }
}

// 접근 방법
DataSourceClass.Datasource.baseUrl
```

👉 클래스 내부에 object를 선언할 수 있다

👉 [ 접근 방법 ] 처럼 객체 이름까지 명시해서 접근해야 한다

<br>

2. Companion Object

```kotlin
// 예제 2
class DataSourceClass {
    companion object {
        var baseUrl: String = "google.com"
      
        fun printBaseUrl() {
            println("baseUrl is : $baseUrl")
        }
    }
}

// 접근 방법
DataSourceClass.baseUrl
```

👉 companion object를 사용하면 객체 이름 없이 바로 접근할 수 있다

👉 코드가 간결하고 자연스럽다

