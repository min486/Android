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

## 🔥 Collection (컬렉션) - list, map, set

### Collection

> 컬렉션은 수집이라는 단어 의미 그대로 다수의 객체를 수집해놓은 객체

- List
  - 인덱스를 통해 요소에 접근할 수 있는 순서가 보장된 컬렉션
  - 중복 요소가 허용된다
- Map
  - key - value 쌍을 모아놓은 컬렉션
  - 모든 키는 중복되지 않으며 하나의 값에만 매핑된다. 값은 중복이 허용된다
- Set
  - 순서를 보장하지 않고 각 요소가 고유하다는 것을 보장하는 컬렉션
  - 순서를 보장하지 않기 때문에 인덱스를 통해 요소에 접근할 수 없다

<br>

### Collection 종류

- mutable (가변형)

  : 요소 값들을 추가하거나 변경할 수 있다

  👉 변경가능한 컬렉션을 사용하려면 앞에 mutable로 써야 한다

- immutable (불변형)

  : 데이터를 한 번 할당하면 읽기 전용이 된다 (변경 X)

  👉 기본적으로 제공하는 것이 immutable

<img src="../README.assets/collection.png" alt="collection" align="left" style="zoom: 67%;" />

### Collection 예제

- List

```kotlin
fun main() {
  val list = mutableListOf(1,2,3,4,5)  // 변경 가능
  list.add(6)
  list.addAll(listOf(7,8,9))
  
  val list2 = listOf(1,2,3,4)  // 변경 불가

  // 한가지 타입 뿐만 아니라 다양한 타입 넣을 수 있다
  val diverseList = listOf(1, "안녕", 1,78, true)

  // 여러가지 확장함수를 사용하면 반복문을 대신할 수 있다
  > list2.map { it * 10 }.joinToString("/")
  >> 10/20/30/40
}
```

- Map

```kotlin
fun main() {
  val map = mutableMapOf((1 to "안녕"), (2 to "hello"))  // 변경 가능
  map[3] = "응"
  map[100] = "아니"
  
  val map2 = mapOf((1 to "안녕"), (2 to "hello"))  // 변경 불가
}
```

✅ 컬렉션을 잘 사용하려면 안에 있는 함수들을 잘 학습해야 한다!!
