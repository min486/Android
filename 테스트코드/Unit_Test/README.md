<div align="center">
  <p>
    <img src="../README.assets/android.png">
  </p>
  <br>
  <h2>테스트 코드</h2>
  <p>테스트 코드 관련 내용 정리</p>
  <br>
  <br>
</div>





## 🔥 테스트 코드

### Unit Test (단위 테스트)

- 가장 작은 코드 단위(함수, 클래스 등)를 독립적으로 테스트하는 코드
- 예 : 어떤 함수가 특정 입력을 받았을 때 올바른 출력을 내는지를 확인

```kotlin
fun add(a: Int, b: Int): Int {
    return a + b
}
```

```kotlin
import org.junit.Test
import org.junit.Assert.*

class MathTest {
  
  	@Test
    fun addition_isCorrect() {
        val result = add(2, 3)
        assertEquals(5, result)
    }
}
```
