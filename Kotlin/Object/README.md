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

## 🔥 Object / Companion Object

### 싱글턴 패턴 (Singleton Pattern)

> 클래스의 인스턴스가 오직 하나만 생성되는 것

<br>

보통 클래스를 선언한 후 객체를 여러개 만들 수 있다

하지만 싱글턴 패턴이 적용된 클래스의 경우 인스턴스는 하나만 생성 되고, 여러곳에서 사용하는 경우 같은 인스턴스를 사용하게 된다

<br>

### Object

코틀린에서는 싱글턴 패턴을 Object 라는 키워드를 통해서 구현할 수 있다

```kotlin
fun main(args: Array<String>) {
  
    // 기존에는 아래처럼 객체를 생성하면 2개의 객체는 다른 것이였음
    val car = Car()
    val car2 = Car()
  
    // 하지만 Object의 경우 객체가 전체 프로그램에서 하나만 만들어지기 때문에 바로 사용가능
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

- 싱글톤을 만들 수 있는 키워드
- 생성자 사용 불가
- 프로퍼티, 메서드, 초기화 블록 사용 가능
- 다른 클래스, 인터페이스 상속 받을 수 있다

<br>

### Companion Object

> 클래스 안에 들어있는 object

```kotlin
fun main(args: Array<String>) {
  
    // companion object를 사용하기 때문에, Dinner 객체 생성 없이 바로 사용가능
    println(Dinner.MENU)  // pasta
    Dinner.eatDinner()  // Today's Menu is pasta
  
    // launch는 companion object 안에 들어가 있지 않아서
    Dinner.launch  // 접근불가
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

- 클래스 하나에 companion object 하나만 사용가능

- companion object에 이름을 붙여도 되고 안 붙여도 된다
- object 키워드와 같은 역할

<br>

### Object와의 차이점

- 클래스 내부에 있는 object를 굳힌 형태인 것
- companion object 내부에 있는 변수, 메소드에 대한 접근을 더 자연스럽게 보이도록 한다

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

// 접근 1
DataSourceClass.Datasource.baseUrl
```

👉 클래스 내부에 object를 선언할 수 있다. 따라서 접근1 처럼 DataSource 객체의 변수에 접근

<br>

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

// 접근 2
DataSourceClass.baseUrl
```

👉 companion object로 바꿀 수 있다. 이로 인해 클래스 내부 객체에 접근할 때 해당 객체의 이름을 반복적으로 타이핑할 필요가 없다

