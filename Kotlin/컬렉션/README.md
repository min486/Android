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

## 🔥 컬렉션 (Collection)

### 컬렉션

- 코틀린의 컬렉션은 데이터를 효율적으로 관리하기 위한 자료구조
- Immutable(변경 불가능)과 Mutable(변경 가능) 두가지 타입으로 나뉜다

<br>

### 컬렉션의 종류

- Immutable Collection
  
  : 선언 이후 요소를 삽입, 삭제, 수정할 수 없음 (읽기 전용)

```kotlin
val fruitList = listOf("Apple", "Banana")
println(fruitList[0])  // Apple
```

- Mutable Collection

  : 요소를 삽입, 삭제, 수정할 수 있음

```kotlin
val mutableFruitList = mutableListOf("Apple", "Banana")
mutableFruitList[0] = "Melon"
println(mutableFruitList[0])  // Melon
```

<br>

### 주요 컬렉션

코틀린에서 가장 많이 사용되는 컬렉션은 List, Set, Map 이다

| 타입 | 특징                   | 예시                    |
| ---- | ---------------------- | ----------------------- |
| List | 순서 있음, 중복 허용   | listOf("a", "b")        |
| Set  | 순서 없음, 중복 불가   | setOf(1, 2, 2) ➡️ [1, 2] |
| Map  | 키-값 쌍, 키 중복 불가 | mapOf("name" to "min")  |

<br>

### Compose에서의 컬렉션 사용 예시

```kotlin
val items = remember { mutableStateListOf("Apple", "Banana") }

LazyColumn {
    items(items) { item ->
        Text(text = item)
    }
}

// 항목 추가
Button(onClick = { items.add("Melon") }) {
    Text("Add Melon")
}
```

